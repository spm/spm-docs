# Other MATLAB Datatypes
Originally, MATLAB arrays only went up to two dimensions and contained values stored as double precision floating point.
However, the language evolved to work with arrays of more dimensions, with different datatypes.
Other data structures were also introduced, such as ``struct`` and ``cell``, which makes some things easier.

## Representing numbers
### Floating point
Floating point numbers are those that can contain fractions, and are not limited to whole numbers.
These can be double precision or single precision.  
#### Double precision floating point
These are the most accurate representations of floating point numbers, but they need more memory than single precision.
When numerical precision is needed, then double precision is preferred. Each number is represented by 8 bytes of memory.
The arrays we have generated so far have used double precision.  Other representations can be converted to double precision using the ``double`` function.

#### Single precision floating point
This representation of floating point numbers uses four bytes, and is less precise than double precision.
A double precision array can be converted to single precision using the ``single`` function.
One way that we can look at the effects of numerical precision is by adding numbers together.  Try comparing the results of the following:
```matlab
d = [1e-8 1 -1];
s = single(d);

sum(d)
sum(s)
sum(d(3:-1:1))
sum(s(3:-1:1))
```
The ``sum(s)`` command produced a value of zero.  This is because in single precision, a value of 1 is indistinguishable from a value of 1.00000005.

### Integers
Many types of whole number can be represented in MATLAB. These are determined by the number of bytes used to encode them and whether they are signed or unsigned.
Unsigned representations don't allow negative values to be represented, whereas signed representations allow negative values.
Available integer types are:

* **uint8** Unsigned 8 bit integers. One byte. Values can range from 0 to 255.
* **int8** Signed 8 bit integers. One byte. Values can range from -128 to 127.
* **uint16** Unsigned 16 bit integers. Two bytes. Values range from 0 to 65535.
* **int16** Signed 16 bit integers. Two bytes. Values range from -32768 to 32767.
* **uint32** Unsigned 32 bit integers. Four bytes. Values range from 0 to 4294967295.
* **int32** Signed 32 bit integers. Four bytes. Values range from -2147483648 to 2147483647.
* **uint64** Unsigned 64 bit integers. Eight bytes. Values range from 0 to 18446744073709551615.
* **int64** Signed 64 bit integers. Eight bytes. Values range from -9223372036854775808 to 9223372036854775807.

In practice, neuroimagers store most images iin int16, often with a scalefactor in the image headers to convert them to quantitative values.

### Logicals
Locical values are another datatype, which encode "true" or "false". "True" is stored as one, while "false" is stored as zero.
In MATLAB, the main logical operations are:

* ``&``, which indicates the element-wise logical ``and`` operation, where the output is only true if both inputs are true. 
* ``|``, which indicates the element-wise logical ``or`` operation, where the output is true if either input is true. For scalar
* `~`, which indicates the logical ``not`` operation. This takes one input and flips it to the other state.

For single scalar logical values, ``&&`` and  ``||`` are faster way to compute ``and`` or ``or``.
Here are some examples:
```matlab
result = [true true false] & [true false false]
same_result = and([true true false], [true false false])
```

It is important to note that a single ``=`` symbol in MATLAB implies an assignment, where the thing on the left-hand-side (lhs) of the sign takes on the value of the thing on the right-hand-side (rhs).
A different meaning of "equals" is "is equal to", which is denoted by ``==``.
Here, the aim is to see if the thing on the lhs is equal to the thing on the rhs. This returns logical values of 1 to denote "true" or 0 to denote "false:
```matlab
X = [1 2 3; 3 2 1];
selected = X==3
selected = X==1 | X==3
```
Related operators are ``>`` (is greater than), ``<`` (is less than), ``>=`` (is greater than or equal to) and ``<=`` (is less than or equal to).

Sometimes it is useful to have the indices of which elements of an array are true, in which case the ``find`` function can be useful:
```matlab
indices = find(selected)
```
Other functions that can be used on logical arrays include ``all`` and ``any``, where the second argument indicates which dimension to work with. Here are some examples of this:
```matlab
X = [true true false
     true false false];
all(X)
all(X,2)
any(X)
any(X,2)
```

### Char
Character arrays can be very useful. Since the advent of Unicode (to enable emojis and other alphabets), character arrays in MATLAB can contain an extremely diverse range of things, although MATLAB does not allow all of them to be seen (e.g. `char(0x1F60A)`). Character arrays are usually used to represent text, such as filenames. Note that the ``disp`` function can be used to show the values of variables.
```matlab
words = 'hello world';
disp(words)
disp(char(words + 'A' - 'a'))
```


## Higher dimensions
Arrays can be any number of dimensions, which is useful because an fMRI dataset can be thought of as a 4D array.
The ``cat`` function can be useful for manually generating a 3D (or higher dimensional) array from a number of 2D arrays.
The first argument indicates the dimension along which the inputs are concatenated.
```matlab
stack = cat(3, [1 2 3; 4 5 6], [0 0 0; 1 1 1], zeros(2,3))
```

Functions, such as ``zeros``, ``ones`` and ``rand`` also work in higher dimensions.
This example creates a random $8 \times 8 \times 4 \times 10$ array of single precision floating point values:
```matlab
X = randn(8,8,4,10);
```

Indexing of higher dimensional arrays is done in a similar way as for 2D arrays.
If we need the number of dimensions in an array, then the ``ndims`` function can tell us.
```matlab
ndims(X)
```

Sometimes we might want to treat the contents of an image as a vector, in which case the ``reshape`` function can be useful.
With this, the output dimensions are provided either as a series of scalar values, or as a vector.
```matlab
vectors = reshape(X,256,10);
```
This example shows the how numbers from 0 to 255 map to ASCII characters:
```matlab
reshape(char(0:255),32,8)'
```





