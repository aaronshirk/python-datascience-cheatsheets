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
