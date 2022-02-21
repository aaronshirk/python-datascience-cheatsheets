# Matplotlib

## Basic plots with Matplotlib

### Matplotlib line plot

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.show()
```

### Matplotlib scatter plot

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.scatter(year, pop)

plt.show()
```

## Histogram

- Explore dataset
- Get idea about distribution

### Matplotlib example of histogram

```
value = [0, 0.6, 1.4, 1.6, 2.2, 2.5, 2.6, 3.2, 3.5, 3.9, 4.2, 6]

plt.hist(value, bins=3)

plt.show()
```

## Customization

- Matplotlib plots has many options
  - Different plot types
  - Many customizations
- Choice depends on
  - Data
  - Story you want to tell

### Axis labels

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')

plt.show()
```

### Title

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')

plt.show()
```

### Ticks

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')
plt.yticks([0, 2, 4, 6, 8, 10])

plt.show()
```

- _plt.yticks()_ sets the range of the y-axis and can shift the graph

### Ticks(2)

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')
plt.yticks([0, 2, 4, 6, 8, 10],
            [0, 2B, 4B, 6B, 8B, 10B])

plt.show()
```

- The second _yticks()_ argument is the ticks labels for more clarify

# Dictionaries and Pandas

## Dictionaries, Part 1

- Object of key values pairs

```
world = {"afghanistan": 30.55, "albania": 2.77, "algeria": 39.21}

world["albania"]
```

- Easy to index into the dictionary using the key value

## Dictionaries, Part 2

### Recap

- Dictionary keys have to be immutable values
- For example, a "string" is immutable, but a list is not, so a list cannot be a key

### More dictionary examples

`world['sealand'] = 0.00607`

- Set the value of a key; this is an update
- Keys are unique in a dictionary

`'sealand' in world`

- This is a way to check if the key 'sealand' exists in the world dictionary

`del(world['sealand'])`

- _del()_ function will remove a key/value entry from a dictionary

## Pandas, Part 1

### DataFrame from Dictionary

```
dict = {
    "country": ['country1', 'country2', 'country3', 'country4'],
    "capital": ['capital1', 'capital2', 'capital3', 'capital4'],
    "area": [8.514, 17.183, 9.576, 1.221],
    "population": [200.4, 143.5, 1357, 52.98]
}
```

- keys (column labels)
- values (data, column by column)

```
import pandas as pd
brics = pd.DataFrame(dict)
```

### DataFrame from dictionary(2)

- The index valuees in the DataFrame above will be numeric, starting at 0, by default
- You can add index labels as follows:

`brics.index = ['C1', 'C2', 'C3', 'C4']`

### DataFrame from CSV file

`brics = pd.read_csv('path/to/brics.csv')`

- Specify the index column explicitly as follows:

`brics = pd.read_csv('path/to/brics.csv', index_col = 0)`

## Pandas, Part 2

### Index and select data

- Square brackets
- Advanced methods
  - loc
  - iloc

### Column access []

series_var = brics['country']

- Single square brackets returns a pandas Series object
- Series is like a 1D labeled array

### Column access [[]]

dataframe_var = brics[['country']]

- Double square brackets returns a DataFrame object

### Row access []

- Use slice syntax

rows = brics[1:4]

- 1 is inclusive, 4 is exclusive

### Row access loc

`brics.loc['RU']`

- Row as a pandas Series

`brics.loc[['RU', 'IN', 'CH']]`

### Row and Column loc

`brics.loc[['RU', 'IN', 'CH'], ['country', 'capital']]`

- Returns selected rows and columns

`brics.loc[:, ['country', 'capital']`

- Returns all rows and selected columns

### Recap

- Square brackets
  - Column access: `brics[['country', 'capital']]`
  - Row access: only through slicing
- loc (label-based)
  - Row access: `brics.loc[['RU', 'IN', 'CH']]`
  - Column access: `brics.loc[:, ['country', 'capital']]`
  - Row and column access: `brics.loc[['RU', 'IN', 'CH'], ['country', 'capital']]`

### Row access iloc

`brics.iloc[[1,2,3]]`

### Row and column access iloc

`brics.iloc[[1,2,3], [0, 1]]`

`brics.iloc[:, [0, 1]]`

# Logic, Control Flow and Filtering

## Comparison Operators

### Numeric Comparisons

`2 < 3`

`2 == 3`

`2 <= 3`

### Other Comparisons

`'carl' < 'chris'`

`3 < 'chris'`

- This one returns a TypeError

`3 < 4.1`

### Comparators

| Comparator | Meaning                  |
| ---------- | ------------------------ |
| <          | less than                |
| <=         | less than or equal to    |
| >          | greater than             |
| >=         | greater than or equal to |
| ==         | equal                    |
| !=         | not equal                |

## Boolean Operators

### NumPy

- `logical_and`
- `logical_or`
- `logical_not`

`np.logical_and(bmi > 21, bmi < 23)`

- Here `bmi` is a NumPy array
- Returns an NumPy array of True/False values

`bmi[np.logical_and(bmi > 21, bmi < 23)]`

## Filtering pandas DataFrames

### Example Goal

- Select the brics countries with area over 8 million km2

- 3 steps
  - Select the area column
  - Do comparison on area column
  - Use result to select the countries

### Step 1: Get column

- A couple different ways to do this

`brics['area']`

`brics.loc[:, 'area']`

`brics.iloc[:, 2]`

### Step 2: Compare

`is_huge = brics['area'] > 8`

### Step 3: Subset DataFrame

`brics[is_huge]`

### Boolean operators

```
import numpy as np

brics[np.logical_and(brics['area'] > 8, brics['area'] < 10)]
```

# Loops

## while loop

## for loop

## Loop Data Structures Part 1

## Loop Data Structures Part 2
