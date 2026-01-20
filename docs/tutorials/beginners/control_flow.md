# Control Flow

## If/Else
Sometimes we need computers to do something different depending on some answer.
This is when ``if``/``else`` statements come in.
Here is the first very simple example. Notice that the ``if`` statement is balanced with an ``end`` statement.
It is conventional to indent instructions between the ``if`` and the ``end`` in order to make code easier to read.
Different people use different amounts of indenting (or tabs), but 4 spaces is typical.
```matlab
number = input('Input a number between 0 and 10: ');
if number<0 || number>10
    disp('That number is out of range');
end
```

Here's a slightly more complex example, which uses an ``else`` statement that determines what is done when the ``if`` statement is false:
```matlab
number = input('Input a number between 0 and 10: ');
if number<0 || number>10
    disp('That number is out of range');
else
    disp('That number is acceptable.')
end
```

We can also use ``elseif`` statements, as in:
```matlab
number = input('Input a number between 0 and 10: ');
if number<0
    disp('That number is negative');
elseif number>10
    disp('That number is too big.')
else
    disp('That number is acceptable.')
end
```

## For
``for`` statements are used when we want to repeat some operations. Again, they are terminated with an ``end`` statement, and the convention is to use indenting.
This is a simple example:
```matlab
count = 0;
for i=1:10
    count = count + i;
    disp(count)
end
```

Here's a slightly more complicated piece of code for doing matrix multiplication, which is something that we will come to later.
First, we set up two matrices, ``A`` and ``B``:
```matlab
A = randn(3,5);
B = randn(5,4);
```

The following code then checks the dimensions of the matrices, and proceeds to multiply them if it is possible.
The results go into a third matrix called ``C``.
```matlab
if ndims(A)>2 || ndims(B)>2
    error('Arrays have too many dimensions.');
elseif size(A,2) ~= size(B,1)
    error('Dimension mismatch.');
end

M = size(A,1);
K = size(A,2);
N = size(B,2);
C = zeros(M,N);
for m=1:M
    for n=1:N
        for k=1:K
            C(m,n) = C(m,n) + A(m,k)*B(k,n);
        end
    end
end
```

The above is just for illustration. In practice, matrix multiplications should not be done like this in MATLAB.
It is an interpreted language, so it is much more efficient to use built-in functions whenever possible.


## While
Another way to control the flow of a program is to use a ``while`` loop.

```matlab
number = input('Input a number between 0 and 10: ');
while number<0 || number>10
    number = input('Try again. Input a number between 0 and 10: ');
end
disp(number)
```

It is also possible to break out of ``while`` and ``for`` loops using ``break``.
Without the ``break``, the following would be an infinite loop:
```matlab
while true
    number = input('Input a number between 0 and 10: ');
    if number>=0 && number<=10
        break
    end
end
disp(number)
```

## Using Functions
Series of instructions are often bundled together into functions. In MATLAB, these have input arguments and output arguments.
Many functions only return a single output argument. For example, ``sin`` is a built-in function that returns a single output.
In the following example, ``x`` is the input argument, and ``y`` is the output argument.
```matlab
x = 0:0.01:2*pi;
y = sin(x);
plot(x,y)
```

Other functions may return several output arguments, in which case these need to be surrounded by square brackets.
An occasionally useful example might be the following, which does something called singular value decomposition (which we may return to later)
```matlab
X = randn(5,3);
[U,S,V] = svd(X)
```
``disp``, which you encountered earlier, is an example of a function that doesn't have output arguments. Other functions don't have input arguments.  Also, functions can change their behaviour depending on how many input and output arguments are specified. For example:
```matlab
s = svd(X)
```

Help about using a functions can be found by typing ``help`` followed by a space and then the name of the function.

### Element-by-element Functions

Arrays can be added and subtracted, providing their dimensions are compatible. The simplest case is when the arrays are the same size, such as this example:
```matlab
Y = randn(3,4);
X = randn(3,4);
disp(X+Y)
```
Addition and subtraction also work if one of the dimensions is 1.
Try this example:
```matlab
Y = 1:8;
X = (1:3)'*10;
disp(X+Y)
disp(X-Y)
```

Elements of arrays can be multiplied and divided in a similar way, but to distinguish it from matrix multiplication or matrix division, the notation uses ``.*`` or ``./``.
```matlab
disp(X.*Y)
disp(X./Y)
```
Note that ``+``, ``-``, ``.*`` and ``./`` are actually a convenient shorthand for the ``plus``, ``minus``, ``times`` and ``rdivide`` functions.
```matlab
disp(plus(X,Y))
disp(minus(X,Y))
disp(times(X,Y))
disp(rdivide(X,Y))
```

There are a load of other mathematical operations that can also be applied element-by-element, such as ``sin``, ``cos``, ``tan``, ``log`` and ``exp``.


### Other Functions of arrays

Some functions that can be applied to arrays, which don't work independently among the elements, include ``sum``, ``mean``, ``prod`` (product), ``cumsum`` (cumulative sum) and ``cumprod`` (cumulative product).
```matlab
X = rand(2,10);
disp(sum(X,2))
disp(prod(X,2))
disp(cumsum(X,2))
disp(cumprod(X,2))
```

There are also the matrix operations, which will be covered later.


## Creating Functions
You can create your own functions by editing a file, which ends in the prefix ``.m``, that is somewhere in the search path that MATLAB uses.
You can see this search path by typing:
```matlab
path
```
The path can also be viewed and edited via a widget at the top of the MATLAB window.
Clicking ``HOME`` and then the ``Set Path`` widget brings up a user interface for seeing and changing this search path (or at least it does on my computer).

If you save the following test as a file called ``quadsol.m``, then it will give you the solutions to a quadratic of the form ``a*x^2 + b*x + c == 0``, which you may remember from school:
```matlab
function [x1, x2] = quadsol(a, b, c)
% Solution to a*x^2 + b*x + c == 0
% FORMAT [x1,x2] = quadsol(a,b,c)
tmp = sqrt(b.^2 - 4*a.*c);
x1  = (-b + tmp)./(2*a); % First solution
x2  = (-b - tmp)./(2*a); % Second solution
```

The first line of the text defines the input (``a``, ``b`` and ``c``) and output (``x1`` and ``x2``) arguments.
This is followed by a block of lines that begin with ``%``.
Normally any text following a ``%`` will be a comment, but in a ``.m`` file, the first block of comments serves as the documentation.
You can see this documentation by typing:
```matlab
help quadsol
```
Subsequent lines are the computations themselves.
Note that the code should allow ``a``, ``b`` and ``c`` to be arrays - providing their dimensions are compatible.
In the code, ``+`` and ``-`` are used in a straightforward way.
Element-wise multiplications and divisions are done with ``.*`` and ``./``.
The reason for this is to distinguish them from ``*`` and ``/``, which denote matrix multiplications and divisions in MATLAB.
You will learn about these later.
The ``^`` symbol denotes "to the power of", but ".^" is used here in order to do elementwise powers.

The function you have created can be called by (for example):
```matlab
[r1,r2] = quadsol(4,3,-2)
```


## Function Handles
MATLAB also includes a way to create simple functions that return a single argument. This can be done using ``@``.
For example, typing the following into MATLAB will create a function called ``quad``:
```matlab
quad = @(x,a,b,c) a.*x.^2 + b.*x + c;
```

This following gives an example of how this function may be used:
```matlab

x = (-3:0.01:3)';

% Parameters of the quadratic
a =  3;
b =  4;
c = -5;

[x1, x2] = quadsol(a, b, c); % Points where x==0
x_minmax = -b/(2*a); % Value of x that gives the minimum/maximum of a quadratic

plot(x, quad(x,a,b,c), '-',  [x1 x2], quad([x1 x2], a, b, c), 'ro-', x_minmax, quad(x_minmax,a,b,c),'rx')
xlabel('x')
ylabel('3x^2 + 4x - 5')
title('Example Quadratic')
```


