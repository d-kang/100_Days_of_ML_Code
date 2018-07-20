# Notes on Octave

- Table of Contents
  - [Basic Operations](#Basic-Operations)
    - [Arithmetic](#Arithmetic)
    - [Setting a row vector](#Setting-a-row-vector)
    - [Setting a column vector](#Setting-a-column-vector)
    - [Performing operations on Matrices element by element](#Performing-operations-on-Matrices-element-by-element)
    - [Special Matrices](#Special-Matrices)
    - [Colon operator :](#Colon-operator-:)
    - [Native Functions](#Native-Functions)
    - [Help Command](#Help-Command)
  - [Loading & Using Data](#Loading-&-Using-Data)
    -  [Navigation](#Navigation)
      - [Load & Save](#Load-&-Save)
      - [Combining Matrices](#Combining-Matrices)
      - [Size & Length Utility Functions](#Size-&-Length-Utility-Functions)
      - [File Types](#File-Types)

## Basic Operations

### Arithmetic

| function           |                    |
| ------------------ | :----------------: |
| +                  | addition           |
| -                  | subtraction        |
| *                  | multiplication     |
| /                  | division           |
| ** *or* ^          | exponent           |
| .+ .- .* ./ .** .^ | element-by-element |

### Setting a row vector
`r = [1 2 3 4]` or `r = [1, 2, 3, 4]`


### Setting a column vector
`c = [1; 2; 3; 4]`

### Performing operations on Matrices element by element
```
m = [1 2; 3 4]
m .+ m                 # [2 4; 6 9]
m .- m                 # [0 0; 0 0]
m .* m                 # [1 4; 9 12]
m ./ m                 # [1 1; 1 1]
m .** m # or m .^ m    # [1 4; 27 256]
```

### Special Matrices
* ones(n, m) - generates a matrix of n x m of 1's
* zeros(n, m) - generates a matrix n x m or 0's
* eye(n) or eye(n, m) - generates identity matrix
* rand(n, m) - generates n x m matrix of uniformly random elements
* randn(n, m) - generates n x m matrix of normally distributed random elements

```
# ommiting the second param assumes square matrix

ones(3)  # [1 1 1; 1 1 1; 1 1 1]
zeros(3) # [0 0 0; 0 0 0; 0 0 0]
eye(3)      # [1 0 0; 0 1 0; 0 0 1]
rand(3, 1)  # [0.35409; 0.81614; 0.36956]
randn(3, 1) # [-2.87424; 0.86242; -0.33722]
```

### Colon operator :
* The colon operator is great for defining a set of values. In a range:
```
# S = start : stop
S = 1 : 5
[1 2 3 4 5]
```
* Or a set with a certain incremental step:
```
# S = start : step : stop
S = 2 : 2 : 10
[2 4 6 8 10]
```

## Indexing
* Now that we have a vector or matrix how do we access elements?
* Simple bracket notation ()
```
S = [1 2 3 4; 10 20 30 40]

S(1, 1)         # [1]
S(1, [1, 2])    # [1 2]
S(1, [1, 4])    # [1 4]


S(1, 1:2)       # [1 2]
S(1, 1:4)       # [1 2 3 4]
S(1, :)         # [1 2 3 4]
S(2, :)         # [10 20 30 40]

S(2, 2:4)       # [20 30 40]

S(:, :)         # [1 2 3 4; 10 20 30 40]
S(1:1, :)       # [1 2 3 4]
S(1, 1:2:4)     # [1 3]

S(1, 1:10)      # error: S(_,10): but S has size 2x4
```
> The Colon operator works like a select all

## Native Functions
> You can format string using the sprintf() function, which works identically to the C version. Eg sprintf("Pi to 8 decimals: %9.8f", pi)

* Also all math functions are available, eg:

| function |                                   |
| -------- | :-------------------------------: |
| ceil(x)  | rounds up to nearest whole number |
| floor(x) | centered                          |
| round(x) | nearest whole number              |
| max(x)   | heighest value in vecotr          |
| min(x)   | lowest value in vector            |

## Help Command
* This command will display the help for any function in Octave
> `help rand` will bring up the info screen on rand and explain what it does
* You can also use `help help` if you get stuck using help
* The [Octave docs](https://octave.org/doc/v4.2.2/) and wiki are very well detailed and well explained


## Loading & Using Data

### Navigation
* Octave uses a working directory just like a terminal/command line
* Supports most commands from both windows and *nix
* cd - change directory
* ls, dir - list current directory
* pwd - print working directory (where you are)

### Load & Save
* Octave's load function is very easy to use. Accepting mny different data formats. (csv, tabulated, etc)
> `load <filename>` or load("<filename>"). This function loads the data into a vector or matrix
* Octave can also save data very easily
> `save <filename> <variable>`

### Temporary Files
* Sometimes when dealing with large amounts of data, or if another program uses data from Octave you may need a temporary file
* Temporary files are deleted when Ocatave closes

```
f = tmpfile
save f <variable>
load f                # places data back into it's old variable
```

example:
```
v = rand(1, 3)        # [0.35292   0.57954   0.54039]
f = tmpfile
save f v
v = [0]               # v is now [0]
load f
v                     # v is now [0.35292   0.57954   0.54039]

```

### Combining Matrices
* When you start working with segmented data, you can join the data together in Octave with easy to use concatenation

```
A = [1; 2; 3]
B = [4; 5; 6]


C = [A B]
# places vector B to right side of vector A
# [1 4; 2 5; 3 6]
# 2 column matrix


D = [A; B]
# Places vector B at end of vector A
# [1; 2; 3; 4; 5; 6]
# concatenated column vector
```

### Size & Length Utility Functions
* Octave has two utility functions for getting the size or length of a vector or matrix

| function  |                               |
| --------- | :---------------------------: |
| size(m)   | returns a matrix of the size  |
| length(v) | returns the longest dimension |

```
A = [1; 2; 3]
B = [4; 5; 6]
C = [1 4; 2 5; 3 6]
D = [1; 2; 3; 4; 5; 6]

size(A)               # 3  1
size(B)               # 3  1
size(C)               # 3  2
size(D)               # 6  1

length(A)             # 3
length(B)             # 3
length(C)             # 3
length(D)             # 6
length([1 1 1 1 1])   # 5
length([1; 1])        # 2
```


### File Types
* OCtave has no problem importing data in most formats
* Matlab data files
* csv (comma separated)
* dat (Tabulated/Whitespace seaprated data)

### Extra's
* If you just use save `<filename>` it will try to save all variables in the interpreter currently
* Be sure to specify the variable you want to save `save <filename> variable`
* Octave supports many other save options, which you can find in the docs. i.e. binary, matlab

> If you want to save out to just plain text you must add the option `-ascii`

> You can use `addpath('<path>') to add a load path

>`clear` will clear all variables in the interpreter


## Plotting Data

### Types of Graphs Supported
* X-Y plots
* Scatter plots
* Histograms
* Contour
* Polar plots
* Pie charts
* 3D meshes
* many more

### X-Y Plots
* Simple x-y plot y in relation to x

|                |                                                                 |
| -------------- | :-------------------------------------------------------------: |
| plot(y)        | Plots the vector y with the index as x                          |
| plot(x, y)     | Plots vector y in relation to x vector                          |
| plot(x, y, fmt | modifies how the line/points are drawn. Default is - solid line |

> There is also a property, value arguments that can be used to change certain attributes. Check the wiki if you need to use this

```
# initialize y to be a row vector 0 through 20
y = 0:20

# reassign y to be each element to the power of 2
y = y .** 2

# use the plot function to plot y
plot(y)

# an x-y plot will pop up
close
```

> use `hold on` command if the graph doesn't show immedietly

> the `close` command will close the graph

### Scatter
* Much like the x-y plot however only the point will be drawn

|                  |                                                                 |
| ---------------- | :-------------------------------------------------------------: |
| scatter(x, y)    | Plots vector y in relation to x vector                          |
| scatter(x, y, s) | modifies how big the marker size is. Default is 8 points        |

> You can add the optional argument "filled" to fill in the markers (Solid dots vs outlined dots)


```
y = 0:20
y = y .** 2

# initialize x to be a row vector 0 through 20
x = 0:20

# use the scatter function with the variables x and y
scatter(x, y)

# a scatter plot will pop up
close
```
