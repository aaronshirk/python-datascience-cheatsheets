# Transforming DataFrames

## Introducing DataFrames

### Exploring a DataFrame: .head()

- Returns the first few rows of a DataFrame

### Exploring a DataFrame: .info()

Shows:

- Column names
- Data types
- Whether columns have any missing values
- Memory Usage

### Exploring a DataFrame: .shape

- Attribute, not a method
- Returns a tuple - `(# rows, # columns)`

### Exploring a DataFrame: .describe()

- Shows summary statistics on numeric columns
- i.e. mean, std
- count - number of non-missing values in each column

### Components of a DataFrame: .value

- df.values - values of the data in a 2D numpy array
- df.columns - column names (index object)
- df.index - row numbers or row names (index object)

## Sorting and subsetting

### Sorting

`df.sort_values('column_name')`

- sort order is ascending by default

### Sorting in descending order

`df.sort_values('column_name', ascending=False)`

### Sorting by multiple variables

`df.sort_values(['column1', 'column2'])`

`df.sort_values(['column1', 'column2'], ascending=[True, False])`

### Subsetting columns

`df['column1]`

- returns a series object on a single column

### Subsetting multiple columns

`df[['column1', 'column2']]`

- Here the outer and inner square brackets are responsible for different things
  - The outer square brackets subset the dataframe
  - The inner square brackets are creating a list of columns to subset
- This could also be written like this below

```
list_of_cols = ['colum1', 'column2']
df[list_of_cols]
```

### Subsetting rows based on a number

`df[df['column1'] > 50]`

- The expression inside the outer square brackets creates a true/false series object which is then used to subset the rows

### Subsetting rows based on text data

`df[df['column1'] == 'text_string']`

### Subsetting rows based on dates

`df[df['date_of_birth'] > '2015-01-01']`

- dates are in quotes and written as YYYY-MM-DD

### Subsetting based on multiple conditions

```
is_lab = dogs['breed'] == 'Labrador'
is_brown = dogs['color'] == 'Brown'
dogs[is_lab & is_brown]
```

### Subsetting using .isin()

```
is_black_or_brown = dogs['color'].isin(['Black', 'Brown])
dogs[is_black_or_brown]
```

## New columns

### Adding a new column

`dogs['height_m'] = dogs['height_cm'] / 100`

### Adding a column based on multiple DataFrame columns

`dogs['bmi'] = dogs['weight_kg'] / dogs['height_m'] ** 2`

### Multiple Manipulations

```
bmi_lt_100 = dogs[dogs['bmi'] < 100]
bmi_lt_100_height = bmi_lt_100.sort_values('height_cm', ascending=False)
bmi_lt_100_height[['name', 'height_cm', 'bmi']]
```

# Aggregating DataFrames

## Summary statistics

### Summarizing numerical data

`dogs['height_cm'].mean()`

- .median()
- .mode()
- .min()
- .max()

### Summarizing dates

- Oldest

`dogs['date_of_birth'].min()`

- Youngest

`dogs['date_of_birth'].max()`

### The .agg() method

```
def pct30(column):
    return column.quantile(0.3)

dogs['weight_kg'].agg(pct30)
```

### Summaries on multiple columns

```
dogs[['weight_kg', 'height_cm']].agg(pct30)
```

### Multiple summaries

```
dogs[['weight_kg', 'height_cm']].agg(pct30, pct40)
```

### Cumulative sum

`dogs['weight_kg'].cumsum()`

- .cummax()
- .cummin()
- .cumprod()

## Counting

### Dropping duplicate names

`df.drop_duplicates(subset='column1')`

### Dropping duplicate pairs

`df.drop_duplicates(subset=['column1', 'column2'])`

### Counting number of a column

`df['column1'].value_counts()`

`df['column1'].value_counts(sort=True)`

- Sort puts the larger count values towards the top of the results

`df['column1'].value_counts(normalize=True)`

- _normalize_ turns counts into proportions of the total

## Grouped summary statistics

### Grouped summaries

`dogs.groupby('color')['weight_kg'].mean()`

### Multiple Grouped summaries

`dogs.groupby('color')['weight_kg'].agg([min, max, sum])`

### Grouping by multiple variables

`dogs.groupby(['color', 'breed'])['weight_kg'].mean()`

### Many groups, many summaries

`dogs.groupby(['color', 'breed'])[['weight_kg', 'height_cm']].mean()`

## Pivot tables

### Groupby compared to pivot table

#### Groupby

`dogs.groupby('color')['weight_kg'].mean()`

#### pivot_table

`dogs.pivot_table(values='weight_kg', index='color')`

- _values_ is the column that you want to calculate summary statistics
- _index_ is the column that you want to group by
- by default .pivot_table() takes the mean of each group

### Different statistics

```
import numpy as np
dogs.pivot_table(values='weight_kg', index='color', aggfunc=np.median)
```

### Multiple statistics

```
import numpy as np
dogs.pivot_table(values='weight_kg', index='color', aggfunc=[np.median, np.mean])
```

### Pivot on two variables

#### Groupby

`dogs.groupby(['color', 'breed'])['weight_kg'].mean()`

#### pivot_table

`dogs.pivot_table(values='weight_kg', index='color', columns='breed')`

### Fill missing values in pivot tables

`dogs.pivot_table(values='weight_kg', index='color', columns='breed', fill_value=0)`

### Summing with pivot tables

`dogs.pivot_table(values='weight_kg', index='color', columns='breed', fill_value=0, margins=True)`

- Last row and column of the pivot table contain the mean of each column or row, not including missing values

# Slicing and Indexing DataFrames

## Explicit Indexes

### Setting a column as an index

`dog_ind = dogs.set_index('name')`

- Moves a column from the body to the index

### Removing an index

`dog_ind.reset_index()`

### Dropping an index

`dog_ind.reset_index(drop=True)`

- This removes the index column

### Indexes make subsetting simpler

#### Non-index subset example

`dogs[dogs['name'].isin(['Bella', 'Stella'])]`

#### Index subset using loc

`dogs_ind.loc[['Bella', 'Stella']]`

### Index values don't need to be unique

```
dogs_ind2 = dogs.set_index('breed')
dogs_ind2.loc['Labrador']
```

- If breed column has multiple Labradors, then all rows will be returned

### Multi-level indexes a.k.a. hierarchical indexes

`dogs_ind2 = dogs.set_index(['breed', 'color'])`

#### Subset the outer level of the index with a list

`dogs_ind3.loc[['Labrador', 'Chihuahua']]`

#### Subset the inner level with a list of tuples

`dogs_ind3.loc[[('Labrador', 'Brown'), ('Chihuahua', 'Tan')]]`

### Sorting by index values

`dogs_ind3.sort_index()`

- By default it sorts from outer index to inner, in ascending order

### Controlling sort_index

`dogs_ind3.sort_index(level=['color', 'breed'], ascending=[True, False])`

### Index have some downsides

- Index values are just data, and here we're storing data in multiple forms
- Indexes violate "tidy data" principles
- You need to learn two syntaxes, one for columns, and another for indexes

# Creating and Visualizing DataFrames

## Visualizing your data

### Histograms

- Show the distribution of a variable

```
import matplotlib.pyplot as plt

dog_pack['height_cm'].hist()

plt.show()
```

### Change number of bars or 'bins'

```
import matplotlib.pyplot as plt

dog_pack['height_cm'].hist(bins=20)

plt.show()
```

### Bar plots

- Show the relationship between a categorical and numeric variable (i.e. breed and weight)\

```
avg_weight_by_breed = dog_pack.groupby('breed')['weight_kg].mean()

avg_weight_by_breed.plot(kind='bar')

plt.show()
```

### Bar plot with title

```
avg_weight_by_breed = dog_pack.groupby('breed')['weight_kg].mean()

avg_weight_by_breed.plot(kind='bar', title='Mean Weight by Dog Breed')

plt.show()
```

### Line plots

- Great for showing changes in numeric variables over time

```
sully.plot(x = 'date', y = 'weight_kg', kind='line')

plt.show()
```

### Line plots - rotate axis labels

```
sully.plot(x = 'date', y = 'weight_kg', kind='line', rot=45)

plt.show()
```

### Scatter plots

- Great for showing the relationship between two numeric variables

```
dog_pack.plot(x = 'height_cm', y='weight_kg', kind='scatter')

plt.show()
```

### Layering plots

```
dog_pack[dog_pack['sex'] == 'F']['height_cm].hist()

dog_pack[dog_pack['sex'] == 'M']['height_cm].hist()

plt.show()
```

### Adding a legend

```
dog_pack[dog_pack['sex'] == 'F']['height_cm].hist()

dog_pack[dog_pack['sex'] == 'M']['height_cm].hist()

plt.legend(['F', 'M'])

plt.show()
```

### Transparency

```
dog_pack[dog_pack['sex'] == 'F']['height_cm].hist(alpha=0.7)

dog_pack[dog_pack['sex'] == 'M']['height_cm].hist(alpha=0.7)

plt.legend(['F', 'M'])

plt.show()
```

## Missing values

### Detecting missing values

`dogs.isna()`

- Get a boolean value for each value in the DataFrame indicating whether values is missing or not
- This isn't very useful for a lot of data

### Detecting any missing values

`dogs.isna().any()`

- Returns a boolean for each column, indicating if there are any missing values in the column

### Counting missing values

`dogs.isna().sum()`

- Taking the sum of booleans is the same as counting the number of True(s)

### Plot missing values

```
import matplotlib.pyplot as plt

dogs.isna().sum().plot(kind='bar')

plt.show()
```

### Removing missing values

`dogs.dropna()`

- Removes rows with missing data
- Can lose a lot of data by doing this though

`dogs.fillna(0)`

- Fills any NaN(s) with 0
