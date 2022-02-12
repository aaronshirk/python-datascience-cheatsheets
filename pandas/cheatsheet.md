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

## Using merge_asof()

## Selecting data with query()

## Reshaping data with melt()
