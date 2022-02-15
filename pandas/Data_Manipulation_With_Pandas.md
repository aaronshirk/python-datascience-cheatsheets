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

# Aggregating DataFrames

# Slicing and Indexing DataFrames

# Creating and Visualizing DataFrames
