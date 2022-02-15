# Data Merging Basics

## Inner Join

### Inner Join

`df3 = df1.merge(df2, on='column1')`

- Merges df1 and df2 together, performing an inner join on _column1_
- _column1_ must be common between the two tables
- Only returns rows that have matching values in both tables

### Example merge on multiple columns

- It's possible to merge on multiple columns

`df3 = df1.merge(df2, on=['column1', 'column2'])`

### Suffixes

- Columns with the same name in both tables will automatically be given a suffix to distinguish them
- Can also use the _suffixes_ argument to control the names

```
df3 = df1.merge(df2, on='column1', suffixes=('_col1', '_col2'))
```

## One to many relationships

- One-To-Many = Every row in left table is related to one or more rows in the right table

### One-to-many example

```
df3 = df1.merge(df2, on='column1', suffixes=('_col1', '_col2'))
```

- Syntax is the same as one-to-one, pandas handles this automatically

## Merging multiple DataFrames

### Merging multiple tables

```
df4 = df1.merge(df2, on=['column1', 'column2']) \
                    .merge(df3, on='column3', suffixes=['_df2', '_df3'])
```

- This pattern can be continued with more tables

# Merging Tables with Different Join Types

## Left join

- Returns all rows in the left table, and only those rows in the right table that match

### Merge with left join

`df3 = df1.merge(df2, on='column1', how='left')`

- Note: the number of rows returned in a left join, one-to-one is always the same number of rows in the left table

## Other joins

### Right Join

- Returns all rows in the right table, and only those rows in the left table that match

### Merge with right join

```
df3 = df1.merge(df2, how='right',
                left_on='column1', right_on='column2')
```

### Outer join

- Retuns all the rows in both tables, regardless of whether there is a match

### Merge with outer join

```
df3 = df1.merge(df2, on='column1', how='outer',
                suffixes=('_df1', '_df2'))
```

## Merging a table to itself

### Example self-join

```
df2 = df1.merge(df1, left_on='column2', right_on='id',
                suffixes=('_org', '_seq'))
```

### Self-join with left join type

```
df2 = df1.merge(df1, left_on='column2', right_on='id',
                how='left' suffixes=('_org', '_seq'))
```

### When to use self-join

- Hierarchical relationships
- Sequential relationships
- Graph data

- Note: self-joins can use the different merge types already discussed (i.e. inner, left, right, outer)

## Merging on indexes

- The _on_ argument of the merge() method will use an index column
- The merge() method automatically adjusts to using the index, otherwise it's much the same as non-index merges

### Example merge on index

```
df3 = df1.merge(df2, on='id', how='left')
```

- Here the _on_ argument = 'id', where 'id' is an index column called 'id'

### Multi-index merge

```
df3 = df1.merge(df2, on=['column1', 'column2'])
```

### Index merge with left_on and right_on

```
df3 = df1.merge(df2, left_on='column1', left_index=True,
                    right_on='column2', right_index=True)
```

# Advanced Merging and Concatenating

## Filtering Joins

Filter observations from a table based on whether or not they match an observation in another table

### Semi joins

- Returns the intersection (like an inner join)
- Returns only columns from the left able and _not_ the right
- No duplicates

1. Generate a filter using the 'isin' method
   This returns a series object

   `filter = table1['column'].isin(table2['column'])`

2. Filter rows of the table using the filter

   `result = table1[filter]`

### Anti join

Anti-join is basically a three step process

1.  Merge two related tables as a left join, with an indicator to facilitate filtering

        empl_cust = employees.merge(top_cust, on='srid', how='left', indicator=True)

2.  Select the srid column where '\_merge' column is left_only; the result is a series filter

        srid_list = empl_cust.loc[empl_cust['_merge'] == 'left_only', 'srid']

3.  Finally, filter your main table by using the 'isin' method

        print(employees[employees['srid'].isin(srid_list)])

## Concatenate DateFrames together vertically

use pandas `.concat()` with `axis=0`

### Basic concatenation

    pd.concat([table1, table2, table3])

_indexes from the tables are preserved_

#### Ignore the index

    pd.concat([table1, table2, table3], ignore_index=True)

#### Setting labels to original tables

    pd.concat([table1, table2, table3], ignore_index=False, keys=['jan', 'feb', 'mar'])

- _creates a double index_
- _ignore_index must be False for this to work_

#### Concatenate tables with different column names

    pd.concat([table1, table2], sort=True)

- _will sort the column names_

#### Using join='inner' argument

    pd.concat([table1, table2], join='inner')

- _will only include column names common to both tables_
- _default value of 'join' is 'outer'_
- _sort argument has no affect with join='inner'_

#### Using append method

- `append()` is a simplified version of `concat()`
- supports: `ignore_index` and `sort`
- does not support: `keys` and `join`

  - Always `join = outer`

    table1.append([table2, table3], ignore_index=True, sort=True)

## Verifying data integrity on merge and concatenate

- Possible merge issues
  - Unintentional one-to-many, many-to-many, etc
- Possible concatenating issues
  - Duplicate records possibly introduced

### Validating Merges

`.merge(validate=None)`

- Checks if merge is of a specified type

  - 'one-to-one'
  - 'one-to-many'
  - 'many-to-one'
  - 'many-to-many'

`table1.merge(table2, on='column', validate='one-to-one')`

- If data is valid, then no issues
- If it detects invalid merge type, it raises MergeError

### Verifying concatenations

`.concat(verify_integrity=False)`

- Checks whether the new concatenated index contains duplicate records
- default value is False

`pd.concat([table1, table2], verify_integrity=True)`

# Merging Ordered and Time-Series Data

## Using merge_ordered()

- Useful for ordered or time-series data
- Like a regular merge, but with an outer join by default, and it's ordered

### Comparing methods

| _merge()_ method                                                         | _merge_ordered()_ method                                                 |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| Columns to join on<br> - on, left_on, right_on                           | Columns to join on<br> - on, left_on, right_on                           |
| Type of join<br> - how (left, right, inner, outer)<br> - _default_ inner | Type of join<br> - how (left, right, inner, outer)<br> - _default_ outer |
| Overlapping column names<br> - suffixes                                  | Overlapping column names<br> - suffixes                                  |
| Calling the method<br> - table1.merge(table2)                            | Calling the method<br> - pd.merge_ordered(table1, table2)                |

### Normal Example

```

import pandas as pd
pd.merge_ordered(table, table2, on='column', suffices=('\_sux2', '\_suf2'))

```

### Forward Fill

- Useful for handling missing data or values

```

pd.merge_ordered(table, table2, on='column',
suffixes=('\_sux2', '\_suf2'),
fill_method='ffill')

```

## Using merge_asof()

- Provides a fuzzy search
- Similar to merge_ordered left join
  - similar features as merge_ordered
- Matches on the nearest key column and not exact matches
- Default match is "backwards" - closest value in right table that is <= the value in left table

### Normal example

```

pd.merge_asof(table1, table2, on='date_time', suffixes=('\_suf1', '\_suf2'))

```

### merge_asof() example with direction

- closest value in the right table that is >= the value in the left table

```

pd.merge_asof(table1, table2, on='date_time',
suffixes=('\_suf1', '\_suf2',
direction='forward'))

```

### When to use

- Data sample from a process and values don't line up exactly
- Developing a training set

## Selecting data with query()

- Accepts an input string
  - Input string used to determine what rows are returned
  - Input string is similar the SQL statement after a _WHERE_ clause

### Querying on a single condition

`df1.query('nike >= 90')`

### Querying on multiple conditions - "and", "or"

`df1.query('nike > 90 and disney < 140)`

`df1.query('nike > 44 or disney < 64)`

### Using .query() to select text

`df1.query('column1=="disney" or (column1=="nike" and column2 < 90)')`

- Here the query itself is inclosed in (') quotes, and the string to compare is in (") quotes

## Reshaping data with melt()

- Wide vs long format data
  - wide tends to be easier for humans to read
  - long format tends to be better for computers
  - The melt() method allows us to unpivot a table from wide to long format

### Example of .melt()

`df2 = df1.melt(id_vars=['column1', 'column2'])`

- _id_vars_ are the columns in the original data set that we don't want to change

### Melting with value_vars

`df2 = df1.melt(id_vars=['column1', 'column2'], value_vars=['column3', 'column4'])`

- _value_vars_ are the columns we want to unpivot
- order of _value_vars_ is reflected in the output

### Melting with column names

```

df2 = df1.melt(id_vars=['column1', 'column2'],
value_vars=['column3', 'column4'],
var_name=['name1'], value_name='name2')

```

```

```
