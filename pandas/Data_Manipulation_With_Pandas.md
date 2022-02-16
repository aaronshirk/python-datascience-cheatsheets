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

# Slicing and Indexing DataFrames

# Creating and Visualizing DataFrames
