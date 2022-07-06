# Introduction and flat files

## Importing entire text files

```
# Open a file: file
file = open('moby_dick.txt', 'r')

# Print it
print(file.read())

# Check whether file is closed
print(file.closed)

# Close file
file.close()

# Check whether file is closed
print(file.closed)
```

## Importing text files line by line

```
# Read & print the first 3 lines
with open('moby_dick.txt') as file:
    print(file.readline())
    print(file.readline())
    print(file.readline())
```

## Using NumPy to import flat files

```
# Import package
import numpy as np

# Assign filename to variable: file
file = 'digits.csv'

# Load file as array: digits
digits = np.loadtxt(file, delimiter=',')
```

## Customizing your NumPy import

There are a number of arguments that np.loadtxt() takes that you'll find useful:

  - *delimiter* changes the delimiter that loadtxt() is expecting.
    - You can use ',' for comma-delimited.
    - You can use '\t' for tab-delimited.
  - *skiprows* allows you to specify how many rows (not indices) you wish to skip
  - *usecols* takes a list of the indices of the columns you wish to keep.

```
  # Import numpy
import numpy as np

# Assign the filename: file
file = 'digits_header.txt'

# Load the data: data
data = np.loadtxt(file, delimiter='\t', skiprows=1, usecols=[0,2])

# Print data
print(data)
```

## Importing different datatypes with `np.loadtext()`:

Header with text can cause issues because it's a string.

Couple options are to:

1. set dtype = str
```
# Assign filename: file
file = 'seaslug.txt'

# Import file: data
data = np.loadtxt(file, delimiter='\t', dtype=str)

# Print the first element of data
print(data[0])
```

2. skip header row
```
# Import data as floats and skip the first row: data_float
data_float = np.loadtxt(file, delimiter='\t', dtype=float, skiprows=1)

# Print the 10th element of data_float
print(data_float[9])
```


## Working with mixed datatypes (1)

```
data = np.genfromtxt('titanic.csv', delimiter=',', names=True, dtype=None)
```

Here, the first argument is the filename, the second specifies the delimiter , and the third argument names tells us there is a header. Because the data are of different types, data is an object called a structured array. Because numpy arrays have to contain elements that are all the same type, the structured array solves this by being a 1D array, where each element of the array is a row of the flat file imported. You can test this by checking out the array's shape in the shell by executing np.shape(data).

## Working with mixed datatypes (2)

`np.recfromcsv()`  is like `np.genfromtext()`, and it has defaults `delimiter=','` and `names=True` and `dtype=None`

```
# Assign the filename: file
file = 'titanic.csv'

# Import file using np.recfromcsv: d
d = np.recfromcsv(file)
print(type(d))

# Print out first three entries of d
print(d[:3])
```