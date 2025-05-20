# SPM and Python

## Differences between MATLAB and Python

Data structures available in MATLAB behave differently from those often
used in Python.

In MATLAB:

- MATLAB uses 1-indexing; 1 denotes the first element in a list.
- Arrays and scalars are instances of the same type (such as `single`
  or `double`). Scalars are arrays with shape `[1 1]`.
- Arrays have an infinite number of singleton dimensions. In other words, an
  array with size `[m n]` and an array with size `[m n 1 1]` are exactly
  identical.
- When an operator acts on arrays with different shapes, these implicit
  trailing dimensions mean that arrays are broadcasted to the right. For
  example, the sum of arrays with shape `[m n]` and `[m 1 k]` is possible
  and results in an array with shape `[m n k]`, as if the first array
  had shape `[m n 1]`.
- The size of an array, returned by `size(a)` always has a length of at
  least 2. That is, a scalar has size `[1 1]`, a column vector has size
  `[n 1]` and a row vector has size `[1 n]`.
- Indexing multidimensional arrays is _explicit_. If the number of indexed
  dimensions is smaller than the number of array dimensions, the remaining
  (rightmost) dimensions are flattened by the indexing operation.
- By default, literals have type `double`, unless they are explicitly
  cast to another data type. That is, `1` has type `double`, even if
  no decimal separator is used.
- Cells (which can contain elements of any type), can be cell _arrays_,
  with any number of dimensions. A one dimensional cell is a cell array
  with size `[1 n]`.
- Similary, structures can be structure _arrays_. A scalar structure is
  a structure array with size `[1 1]`.
- The fields of a structure are accessed using the dot notation (`x.field`).
  When the field of a structure array is accessed, a series of individual
  values are returned, which can be stored in a cell
  (`s(3,3).a = 1; [c{1:9}] = s.a`).
- MATLAB structures can be implicitly defined and reshaped; assigning an
 element out of bounds implicitly resizes a numeric (or cell or structure)
 array.

In Python:

- Python uses 0-indexing; 0 denotes the first element in a list.
- Python defines builtin scalar types `bool`, `int`, `float` and `complex`.
  The underlying `C` data type is plarform specific, but generally maps
  to `bool`, `int64`, `float64` and `complex128`.
- Python does not defines builtin arrays, but the widely used NumPy package
  does. All NumPy arrays have the same type (`np.ndarray`), and the data
  type of their elements is an attribute of a given array instance.
- The shape of a NumPy array is one of its attributes. Arrays can have an
  empty shape (in which case it is equivalent to a scalar for most
  purposes), or a shape with any number of dimensions. Dimensions are
  _explicit_: arrays with shape `[m, n]` and `[m, n, 1, 1]` behave
  differently.
- When an operator acts on arrays with different number of dimensions,
  arrays are broadcasted to the left. For example, the sum of arrays with
  shape `[m n]` and `[k 1 n]` is possible and results in an array with
  shape `[k m n]`, as if the first array had shape `[1 m n]`.
- Indexing multidimensional arrays is _implicit_: non-indexed dimensions
  are preserved (`a[i, j]` and `a[i, j, :, :]` are equivalent if `a` has
  four dimensions).
- Literals that have a decimal separator are of type `float` (_e.g._, `1.`),
  whereas those that do not are of type `int` (_e.g._, `1`).
- Lists can contain elements of any type, and are one-dimensional.
- Dictionaries are accessed with the indexing notation (`x["field"]) and
  are always scalar.

## The SPM-Python type system

Internally, SPM makes advanced use of MATLAB types; it behaves differently
whether its inputs are cells of structures or structures or cells, and
dimensions have a semantic meaning. This makes communicating between SPM
and Python challenging. To enable this communication, we have
implemented a dedicated Python type system that can easily be converted
to (and from) MATLAB types. Out aim, when designing this type system, was
to create a type system with a pythonic feels, and that would not throw
off Python users. At the same type, we have tried to reach a one-to-one
mapping between this new type system and MATLAB types, and support an
extended set of features that ease the translation of MATLAB scripts into
Python. Therefore:

- We implement `Array`, `Cell` and `Struct` types that can all behave as
  arrays, as well as scalars (`Array`, `Struct`) or one dimensional lists
  (`Cell`).
- All types are based off (and inherit) `np.ndarray`.
- We use numpy-like indexing (0-indexing, implicit dimension indexing,
  broadcasting to the left).
- We support implicit resizing when indexing out-of-bounds.
- Structure fields can be indexed using either dot (`x.field`) or bracket
  (`x["field"]`) indexing.
- A scalar (0-dimensional) `Struct` implements the `Mapping` protocol.
- A row (1-dimensional) `Cell` implements the `List` protocol.

### Examples

The `Array` class is mostly inter-changeable with the `numpy.ndarray` class,
and pure NumPy arrays can be passed to SPM functions that expect a numeric
array. The `Array` class adds implicit indexing, such that the following
(self-explanatory) snippet works:

```python
x = Array([0])  # Define an array with shape [0] -- it is empty
x[1] = 1        # Assign value 1 at index 3. Intermediate indices are filled with zeros
print(x)        # Prints: [0.0, 1.0]
print(repr(x))  # Prints: Array([0., 1.])
```

!!! failure
    Note the, contrary to matlab, only one-dimensional implicit indexing
    is supported. It is therefore not possible to do
    ```python
    x = Array([0, 0])
    x[3, 3] = 1
    ```

Implicit indexing is also supported by types `Cell` and `Struct`:

```python
c = Cell()
c[1] = 1
print(c)        # Prints: [[], 1]
print(repr(c))  # Prints: Cell([Array([]), 1])

s = Struct()
s[1].field = 1
print(s)        # Prints: [{'field': Array([])}, {'field': 1}]
print(repr(s))  # Prints: Struct([{'field': Array([])}, {'field': 1}])
```

Implicit indexing can be deeply nested, a feature that is commonly used
in MATLAB to write SPM batch jobs. For example this SPM-MATLAB batch line

```matlab
realign_estimate_reslice.matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.quality = 0.9;
```

would be written

```python
realign_estimate_reslice = Struct()
realign_estimate_reslice.matlabbatch(0).spm.spatial.realign.estwrite.eoptions.quality = 0.9
```

in SPM-Python. Note that we use round brackets (`matlabbatch(0).spm`)
instead of square brackets (`matlabbatch[0].spm`) to specify that `matlabbatch`
should be a cell that contains a struct, and not a struct array.

In order to implement this nesting of implicitely indexed structured,
we had to delay the creation of each object in the hierarchy, as the final
type of an object is only known after it has been indexed. To do this,
we introducted a delayed type that is originally generic, and metamorphoses
itself into a `Struct` or `Cell` or `Array` as it is being used. Our logic
is documented in the `DelayedArray` section.

### `Array`

`Array` implements numeric arrays, compatible with matlab arrays.

```python
# Instantiate from size
Array(N, M, ...)
Array([N, M, ...])
Array.from_shape([N, M, ...])

# Instantiate from existing numeric array
Array(other_array)
Array.from_any(other_array)

# Other options
Array(..., dtype=None, order=None, *, copy=None, owndata=None)
```

!!! warning
    Lists or vectors of integers can be interpreted as shapes or as
    numeric arrays to copy. They are interpreted as shapes by the
    `Array` constructor. To ensure that they are interpreted as
    arrays to copy, use `Array.from_any`.

```python
@classmethod
def from_shape(cls: Array, shape=tuple(), **kwargs) -> Array:
    """
    Build an array of a given shape.

    Parameters
    ----------
    shape : list[int]
        Shape of new array.

    Other Parameters
    ----------------
    dtype : np.dtype | None, default='double'
        Target data type.
    order : {"C", "F"} | None, default=None
        Memory layout.

        * "C" : row-major (C-style);
        * "F" : column-major (Fortran-style).

    Returns
    -------
    array : Array
        New array.
    """
    ...


@classmethod
def from_any(cls: Array, other, **kwargs) -> Array:
    """
    Convert an array-like object to a numeric array.

    Parameters
    ----------
    other : ArrayLike
        object to convert.

    Other Parameters
    ----------------
    dtype : np.dtype | None, default=None
        Target data type. Guessed if `None`.
    order : {"C", "F", "A", "K"} | None, default=None
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style);
        * `"A"` : (any) `"F"` if a is Fortran contiguous, `"C"` otherwise;
        * `"K"` : (keep) preserve input order;
        * `None`: preserve input order if possible, `"C"` otherwise.
    copy : bool | None, default=None
        Whether to copy the underlying data.

        * `True` : the object is copied;
        * `None` : the the object is copied only if needed;
        * `False`: raises a `ValueError` if a copy cannot be avoided.
    owndata : bool, default=None
        If `True`, ensures that the returned `Array` owns its data.
        This may trigger an additional copy.

    Returns
    -------
    array : Array
        Converted array.
    """
    ...


@classmethod
def from_cell(cls: Array, other: Cell, **kwargs) -> Array:
    """
    Convert a `Cell` to a numeric `Array`.

    Parameters
    ----------
    other : Cell
        Cell to convert.

    Other Parameters
    ----------------
    dtype : np.dtype | None, default=None
        Target data type. Guessed if `None`.
    order : {"C", "F", "A", "K"} | None, default="K"
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style);
        * `"A"` : (any) `"F"` if a is Fortran contiguous, `"C"` otherwise;
        * `"K"` : (keep) preserve input order.
    owndata : bool, default=None
        If `True`, ensures that the returned `Array` owns its data.
        This may trigger an additional copy.

    Returns
    -------
    array : Array
        Converted array.
    """
    ...
```

### `SparseArray`

`SparseArray` implements sparse arrays.

- If `scipy` is available, it uses a `scipy.sparse.csc_array` backend.
- Otherwise, it uses a dense backend.

```python
# Instantiate from size
SparseArray(N, M, ...)
SparseArray([N, M, ...])
SparseArray.from_shape([N, M, ...])

# Instantiate from existing sparse or dense array
SparseArray(other_array)
SparseArray.from_any(other_array)

# Other options
SparseArray(..., dtype=None, *, copy=None)
```

!!! warning
    Lists or vectors of integers can be interpreted as shapes
    or as dense arrays to copy. They are interpreted as shapes
    by the `SparseArray` constructor. To ensure that they are
    interpreted as dense arrays to copy, usse `SparseArray.from_any`.

```python
@classmethod
def from_coo(cls: SparseArray, values, indices, shape=None, **kw) -> SparseArray:
    """
    Build a sparse array from indices and values.

    Parameters
    ----------
    values : (N,) ArrayLike
        Values to set at each index.
    indices : (D, N) ArrayLike
        Indices of nonzero elements.
    shape : list[int] | None
        Shape of the array.
    dtype : np.dtype | None
        Target data type. Same as `values` by default.

    Returns
    -------
    array : SparseArray
        New array.
    """
    ...

@classmethod
def from_shape(cls: SparseArray, shape=tuple(), **kwargs) -> SparseArray:
    """
    Build an array of a given shape.

    Parameters
    ----------
    shape : list[int]
        Shape of the new array.

    Other Parameters
    ----------------
    dtype : np.dtype | None, default='double'
        Target data type.

    Returns
    -------
    array : SparseArray
        New array.
    """
    ...

@classmethod
def from_any(cls: SparseArray, other, **kwargs) -> SparseArray:
    """
    Convert an array-like object to a numeric array.

    Parameters
    ----------
    other : ArrayLike
        object to convert.

    Other Parameters
    ----------------
    dtype : np.dtype | None, default=None
        Target data type. Guessed if `None`.
    copy : bool | None, default=None
        Whether to copy the underlying data.

        * `True` : the object is copied;
        * `None` : the the object is copied only if needed;
        * `False`: raises a `ValueError` if a copy cannot be avoided.

    Returns
    -------
    array : SparseArray
        Converted array.
    """
    ...
```

### `Cell`

`Cell` implements cell arrays, compatible with matlab cells.

```python
# Instantiate from size
Cell(N, M, ...)
Cell([N, M, ...])
Cell.from_shape([N, M, ...])

# Instantiate from existing (cell-like) array (implicitely)
Cell(cell_like)
Cell.from_any(cell_like)

# Other options
Cell(..., order=None, *, copy=None, owndata=None, deepcat=False)
```

A cell is a `MutableSequence` and therefore (mostly) behaves like a list.
It implements the following methods (which all operate along the
1st dimension, if the cell is a cell array):

- `append`        : Append object to the end of the cell
- `clear`         : Empty the cell
- `count`         : Number of occurrences of a value
- `extend`        : Extend list by appending elements from the iterable
- `index`         : First index of a value
- `insert`        : Insert object before index
- `pop`           : Remove and return item at index
- `remove`        : Remove first occurrence of value
- `reverse`       : Reverse the cell in-place
- `sort`          : Sort the list in ascending order in-place

The magic operators `+` and `*` also operate as in lists:

- `a + b`         : Concatenate `a` and `b`
- `a += b`        : Append iterable `b` to `a`
- `a * n`         : Concatenate `n` repeats of `a`
- `a *= n`        : Append `n` repeats of `a` to `a`

Finally, elements (along the first dimension) can be deleted using
`del cell[index]`.

!!! warning
    Lists or vectors of integers can be interpreted as shapes or as
    cell-like objects to copy. They are interpreted as shapes by the
    `Cell` constructor. To ensure that they are interpreted as
    arrays to co~~py, use `Cell.from_any`.

```python
@classmethod
def from_shape(cls: Cell, shape=tuple(), **kwargs) -> Cell:
    """
    Build a cell array of a given size.

    Parameters
    ----------
    shape : list[int]
        Input shape.

    Other Parameters
    ----------------
    order : {"C", "F"} | None, default="C"
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style).

    Returns
    -------
    cell : Cell
        New cell array.

    """
    ...

@classmethod
def from_any(cls: Cell, other, **kwargs) -> Cell:
    """
    Convert a (nested) list-like object to a cell.

    Parameters
    ----------
    other : CellLike
        object to convert.

    Other Parameters
    ----------------
    deepcat : bool, default=False
        Convert cells of cells into cell arrays.
    order : {"C", "F", "A", "K"} | None, default=None
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style);
        * `"A"` : (any) `"F"` if a is Fortran contiguous, `"C"` otherwise;
        * `"K"` : (keep) preserve input order;
        * `None`: preserve input order if possible, `"C"` otherwise.
    copy : bool | None, default=None
        Whether to copy the underlying data.

        * `True` : the object is copied;
        * `None` : the the object is copied only if needed;
        * `False`: raises a `ValueError` if a copy cannot be avoided.
    owndata : bool, default=False
        If `True`, ensures that the returned `Cell` owns its data.
        This may trigger an additional copy.

    Returns
    -------
    cell : Cell
        Converted cell.
    """
    ...
```

### `Struct`

`Struct` implement structure/dictionary arrays, compatible with matlab structs.

```python
# Instantiate from size
Struct(N, M, ...)
Struct([N, M, ...])
Struct.from_shape([N, M, ...])

# Instantiate from existing struct array
# (or list/cell of dictionaries)
Struct(struct_like)
Struct.from_any(struct_like)

# Instantiate from dictionary
Struct(a=x, b=y)
Struct({"a": x, "b": y})
Struct.from_any({"a": x, "b": y})
```

The following field names correspond to existing attributes or
methods of `Struct` objects and are therefore protected. They can
still be used as field names, but only through the dictionary syntax
(`s["shape"]`), not the dot syntax (`s.shape`):

- `ndim -> int`            : number of dimensions
- `shape -> list[int]`     : array shape
- `size -> int`            : number of elements
- `reshape() -> Struct`    : struct array with a different shape
- `keys() -> list[str]`    : field names
- `values() -> list`       : values (per key)
- `items() -> [str, list]` : (key, value) pairs
- `get() -> list`          : value (per element)
- `setdefault()`           : sets default value for field name
- `update()`               : update fields from dictionary-like
- `as_num -> raise`        : interpret object as a numeric array
- `as_cell -> raise`       : interpret object as a cell array
- `as_struct -> Struct`    : interpret object as a struct array
- `as_dict() -> dict`      : convert to plain dictionary
- `from_shape() -> Struct` : build a new empty struct
- `from_any() -> Struct`   : build a new struct by (shallow) copy
- `from_cell() -> Struct`  : build a new struct by (shallow) copy

The following field names are protected because they have a special
meaning in the python language. They can still be used as field names
through the dictionary syntax:

|        |          |          |            |            |           |
| ------ | -------- | -------- | ---------- | ---------- | --------- |
| `as`   | `assert` | `break`  | `class`    | `continue` | `def`     |
| `del`  | `elif`   | `else`   | `except`   | `False`    | `finally` |
| `for`  | `from`   | `global` | `if`       | `import`   | `in`      |
| `is`   | `lambda` | `None`   | `nonlocal` | `not`      | `or`      |
| `pass` | `raise`  | `return` | `True`     | `try`      | `while`   |
| `with` | `yield`  |          |            |            |           |

```python

@classmethod
def from_shape(cls: Struct, shape=tuple(), **kwargs) -> Struct:
    """
    Build a struct array of a given size.

    Parameters
    ----------
    shape : list[int]
        Input shape.

    Other Parameters
    ----------------
    order : {"C", "F"} | None, default="C"
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style).

    Returns
    -------
    struct : Struct
        New struct array.

    """
    ...

@classmethod
def from_any(cls: Struct, other, **kwargs) -> Struct:
    """
    * Convert a dict-like object to struct; or
    * Convert an array of dict-like objects to a struct array.

    Parameters
    ----------
    other : DictLike | ArrayLike[DictLike]
        object to convert.

    Other Parameters
    ----------------
    order : {"C", "F", "A", "K"} | None, default=None
        Memory layout.

        * `"C"` : row-major (C-style);
        * `"F"` : column-major (Fortran-style);
        * `"A"` : (any) `"F"` if a is Fortran contiguous, `"C"` otherwise;
        * `"K"` : (keep) preserve input order;
        * `None`: preserve input order if possible, `"C"` otherwise.
    copy : bool | None, default=None
        Whether to copy the underlying data.

        * `True` : the object is copied;
        * `None` : the the object is copied only if needed;
        * `False`: raises a `ValueError` if a copy cannot be avoided.
    owndata : bool, default=None
        If `True`, ensures that the returned `Struct` owns its data.
        This may trigger an additional copy.

    Returns
    -------
    struct : Struct
        Converted structure.
    """
    ...
```

### `AnyDelayedArray`

A `AnyDelayedArray` is an object that we return when we don't know how
an indexed element will be used yet.

It decides whether it is a Struct, Cell or Array based on the
type of indexing that is used.

In Matlab:

- `a(x,y)   = num`  indicates that `a` is a numeric array;
- `a(x,y)   = cell` indicates that `a` is a cell array;
- `a{x,y}   = any`  indicates that `a` is a cell array;
- `a(x,y).f = any`  indicates that `a` is a struct array;
- `a.f      = any`  indicates that `a` is a struct.

These indexing operations can be chained, so in
`a(x).b.c{y}.d(z) = 2`:

- `a`    is a struct array;
- `b`    is a struct;
- `c`    is a cell;
- `c{y}` is a struct
- `d`    is a numeric array.

In Python, there is only one type of indexing (`[]`). This is a problem as
we cannot differentiate `a{x}.b = y` — where `a` is a cell that contains
a struct — from `a(x).b = y` — where `a` is a struct array.

One solution may be to abuse the "call" operator `()`, so that it returns a
cell. This would work in some situations (`a[x].b = y` is a struct array,
whereas `a(x).b = y` is a cell of struct). However, the statement
`a(x) = y` (which would correspond to matlab's `a{x} = y`) is not valid
python syntax. Furthermore, it would induce a new problem, as cells could
not be differentiated from function handles, in some cases.

Instead, the use of brackets automatically transforms the object into
either:

- a `Struct` (in all "get" cases, and in the "set" context `a[x] = y`,
  when `y` is either a `dict` or a `Struct`); or
- an `Array` (in the "set" context `a[x] = y`, when `y` is neither a
  `dict` nor a `Struct`).

Alternatively, if the user wishes to specify which type the object should
take, we implement the properties `as_cell`, `as_struct` and `as_num`.

Therefore:

- `a[x,y]             = num`    : `a` is a numeric array;
- `a[x,y]             = struct` : `a` is a numeric array;
- `a[x,y].f           = any`    : `a` is a struct array;
- `a(x,y).f           = any`    : `a` is a cell array containing a struct;
- `a.f                = any`    : `a` is a struct.

And explictly:

- `a.as_cell[x,y]     = any`    : `a` is a cell array;
- `a.as_struct[x,y].f = any`    : `a` is a struct array;
- `a.as_cell[x,y].f   = any`    : `a` is a cell array containing a struct;
- `a.as_num[x,y]      = num`    : `a` is a numeric array.
