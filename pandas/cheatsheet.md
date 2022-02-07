## Joins

### Anti - Join

Anti-join is basically a three step process

1. Merge two related tables as a left join, with an indicator to facilitate filtering

   `empl_cust = employees.merge(top_cust, on='srid', how='left', indicator=True)`

2. Select the srid column where '\_merge' column is left_only; the result is a series filter

   `srid_list = empl_cust.loc[empl_cust['_merge'] == 'left_only', 'srid']`

3. Finally, filter your main table by using the 'isin' method

   `print(employees[employees['srid'].isin(srid_list)])`
