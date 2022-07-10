# Best Practices

## Crafting a docstring

```
def count_letter(content, letter):
  """Count the number of times `letter` appears in `content`.

  Args:
    content (str): The string to search.
    letter (str): The letter to search for.

  Returns:
    int

  # Add a section detailing what errors might be raised
  Raises:
    ValueError: If `letter` is not a one-character string.
  """
  if (not isinstance(letter, str)) or len(letter) != 1:
    raise ValueError('`letter` must be a single character string.')
  return len([char for char in content if char == letter])
```

## Retrieving docstrings

By attribute:

```
# Get the "count_letter" docstring by using an attribute of the function
docstring = count_letter.__doc__

border = '#' * 28
print('{}\n{}\n{}'.format(border, docstring, border))
```

By using inspect module

```
import inspect

# Inspect the count_letter() function to get its docstring
docstring = inspect.getdoc(count_letter)

border = '#' * 28
print('{}\n{}\n{}'.format(border, docstring, border))
```

## Keep functions DRY and "Do One Thing"

## Mutable variables and default args

It's generally not a good practice to use mutable variables as default argument to a function.

For example, below the arg `df` is a mutable variable with a dataframe as a default argument

```
def add_column(values, df=pandas.DataFrame()):
  """Add a column of `values` to a DataFrame `df`.
  The column will be named "col_<n>" where "n" is
  the numerical index of the column.

  Args:
    values (iterable): The values of the new column
    df (DataFrame, optional): The DataFrame to update.
      If no DataFrame is passed, one is created by default.

  Returns:
    DataFrame
  """
  df['col_{}'.format(len(df.columns))] = values
  return df
```

Instead set the default to `None`

```
# Use an immutable variable for the default argument
def better_add_column(values, df=None):
  """Add a column of `values` to a DataFrame `df`.
  The column will be named "col_<n>" where "n" is
  the numerical index of the column.

  Args:
    values (iterable): The values of the new column
    df (DataFrame, optional): The DataFrame to update.
      If no DataFrame is passed, one is created by default.

  Returns:
    DataFrame
  """
  # Update the function to create a default DataFrame
  if df is None:
    df = pandas.DataFrame()
  df['col_{}'.format(len(df.columns))] = values
  return df
```

# Context Managers

# Decorators

# More on Decorators

## Print the return type (decorator)

```
def print_return_type(func):
  # Define wrapper(), the decorated function
  def wrapper(*args, **kwargs):
    # Call the function being decorated
    result = func(*args, **kwargs)
    print('{}() returned type {}'.format(
      func.__name__, type(result)
    ))
    return result
  # Return the decorated function
  return wrapper

@print_return_type
def foo(value):
  return value

print(foo(42))
print(foo([1, 2, 3]))
print(foo({'a': 42}))
```

## Example function call counter decorator

```
def counter(func):
  def wrapper(*args, **kwargs):
    wrapper.count += 1
    # Call the function being decorated and return the result
    return func(*args, **kwargs)
  wrapper.count = 0
  # Return the new decorated function
  return wrapper

# Decorate foo() with the counter() decorator
@counter
def foo():
  print('calling foo()')

foo()
foo()

print('foo() was called {} times.'.format(foo.count))
```

## Preserving docstrings when decorating functions

```
from functools import wraps

def add_hello(func):
  # Decorate wrapper() so that it keeps func()'s metadata
  @wraps(func)
  def wrapper(*args, **kwargs):
    """Print 'hello' and then call the decorated function."""
    print('Hello')
    return func(*args, **kwargs)
  return wrapper

@add_hello
def print_sum(a, b):
  """Adds two numbers and prints the sum"""
  print(a + b)

print_sum(10, 20)
print_sum_docstring = print_sum.__doc__
print(print_sum_docstring)
```

## Measuring decorator overhead

Here calling the original wrapped function

```
@check_everything
def duplicate(my_list):
  """Return a new list that repeats the input twice"""
  return my_list + my_list

t_start = time.time()
duplicated_list = duplicate(list(range(50)))
t_end = time.time()
decorated_time = t_end - t_start

t_start = time.time()
# Call the original function instead of the decorated one
duplicated_list = duplicate.__wrapped__(list(range(50)))
t_end = time.time()
undecorated_time = t_end - t_start

print('Decorated time: {:.5f}s'.format(decorated_time))
print('Undecorated time: {:.5f}s'.format(undecorated_time))
```

## Run_n_times - decorators with arguments

A couple different ways to call decorators that take arguments. This is an example of what the definition of a decorator that takes args looks like. This is actually syntactic sugar, and is what is called a "decorator factory" because it first returns a decorator, and then it executes the decorator on the wrapped function.

```
def run_n_times(n):
  """Define and return a decorator"""
  def decorator(func):
    def wrapper(*args, **kwargs):
      for i in range(n):
        func(*args, **kwargs)
    return wrapper
  return decorator
```

1. Call decorator factory with decorator syntax. Here the `run_n_times(10)` returns a decorator

```
# Make print_sum() run 10 times with the run_n_times() decorator
@run_n_times(10)
def print_sum(a, b):
  print(a + b)

print_sum(15, 20)
```

2. Call decorator factory first as a function and get a reference to the new decorator

```
# Use run_n_times() to create the run_five_times() decorator
run_five_times = run_n_times(5)

@run_five_times
def print_sum(a, b):
  print(a + b)

print_sum(4, 100)
```

3. Call the decorator and decorator factory simply as functions without the '@' decorator syntax

```
# Modify the print() function to always run 20 times
print = run_n_times(20)(print)

print('What is happening?!?!')
```
