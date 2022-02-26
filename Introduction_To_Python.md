# Python Basics

## Python

- General purpose; build anything
- Open Source! Free!
- Python packages, also for data science
  - Many applications

## Variables and Types

### Python Types

- float
- int
- str
- bool

# Python Lists

## Python Lists

### Python List

- `[a, b, c]`
- Name a collection of values
- Contain any type

## Subsetting Lists

### List slicing

`[ start : end ]`

- 'start' inclusive; 'end' exclusive

## Manipulating Lists

### Changing list elements

`fam[7] = 1.76`

`fam[0:2] = ['lisa', 1.84]`

### Adding and removing elements

`fam + ['me', 1.54]`

- Concatenates the two lists

`del(fam[2])`

- Deletes the 3rd element from the list

# Functions and Packages

## Functions

### Basic notes

- Piece of reusable code
- Solves a particular task

### Example max()

```
fam = [1.23, 1.68, 1.77, 1.45]
max(fam)
```

- returns 1.77

### round()

`round(1.67, 1)`
--> 1.7

`round(1.67)`
--> 2

`help(round)`

- `round(number)`
- `round(number, ndigits)`

## Methods

- Functions that belong to objects

### list methods

`fam.index('mom')`

- Returns the index of the value 'mom' in the list

`fam.count(1.76)`

- Returns the count of occurences of 1.76 in the list

`fam.append('aaron')`

- Appends the str = 'aaron' on the end of the list

### str methods

`sister = 'liz'`

`sister.capitalize()`

- Returns 'Liz'

`sister.replace('z', 'sa')`

- Returns 'lisa'

# Numpy
