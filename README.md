# Introduction

SQL is popular database language used to query massive datasets. Now you can do the same in Python.

## SELECT

The most simple SQL statement will look like:
```
# Select everything from table1
SELECT *
FROM table1
```
In pandas will look like:
```
df.head()
```
Although, by default, pandas will show only 5 first rows, what in SQL will be equivalent to LIMIT 5 or TOP (5) (depends on DBMS). Also just putting df in terminal will show you first and last 15 rows of the DataFrame.

To select specific columns in SQL we will write the next query:
```
SELECT column1, column2
FROM table1
```
In pandas that will be written like this:
```
df[['column1', 'column2']]
```

## WHERE

Normally we want to select data with a specific criteria and in SQL it will be done using WHERE clause:
```
SELECT column1, column2
FROM table1
WHERE column1 = 2
```
In pandas this same query looks slightly different:
```
df[['column1', 'column2']].loc[df['column1'] == 2]
```
WHERE clause accepts logical operators – pandas too. So this SQL query:
```
SELECT *
FROM table1
WHERE column1 > 1 AND column2 < 25
```
In pandas takes the following form:
```
df.loc[(df['column1'] > 1) & (df['column2'] < 25)]
```
Operator for OR in pandas is '|', NOT - '~'.
```
df.loc[(df['column1'] > 1) | ~(df['column2'] < 25)]
```
SQL WHERE clause also accepts more complex comparisons, for example: LIKE, IN and BETWEEN; pandas is also capable of doing it - just in a different way. Lets create a query where all these operators are used:
```
SELECT *
FROM table1
WHERE column1 BETWEEN 1 and 5 AND column2 IN (20,30,40,50) AND column3 LIKE '%arcelona%'
```
This same query in pandas:
```
df.loc[(df['colum1'].between(1,5)) & (df['column2'].isin([20,30,40,50])) & (df['column3'].str.contains('arcelona'))]
```
More details on these methods can be found in documentation: IN, BETWEEN, LIKE.

## JOIN

Joining tables is a common practice in data scientist's routine and in SQL we do that using 4 different types of JOINS: INNER, LEFT, RIGHT and FULL. The query looks like the following:
```
SELECT t1.column1, t2.column1
FROM table1 t1
INNER JOIN table2 t2 ON t1.column_id = t2.column_id
```
For all joins the syntax is the same, so we just change INNER to any of 3 types that left. In pandas this operation is better to do in 2 steps - first perform join and then select the data we need (although it can be done in one line):
```
df_joined = df1.join(df2, on='column_id', how='inner')
df_joined.loc[['column1_df1', 'column1_df2']]
```
If parameters on and how are not specified, pandas will perform LEFT join using indexes as key columns.

## GROUP BY

Grouping allows us to get some aggregation insights about our data: COUNT, SUM, AVG, MIN, MAX and so on:
```
SELECT column1, count(*)
FROM table1
GROUP BY column1
```
In pandas we also have this flexibility:
```
df.groupby('column1')['column1'].count()
```
We have to specify column name in square brackets to include into results only that column otherwise we will get counts per each column in DataFrame.

If we want to get a sum of all values in column2 grouped by column1 (total sales in every store, for example) and show only those that have reached certain level, lets say more than 1000 units, in SQL we would do the following:
```
SELECT store, sum(sales)
FROM table1
GROUP BY store
HAVING sum(sales) > 1000
```
In pandas we don't have this privilege of HAVING clause, but still we can do that, in two steps: first to group data and then to filter it:
```
df_grouped = df.groupby('store')['sales'].sum()
df_grouped.loc[df_grouped > 1000]
```
## ORDER BY

To sort results in SQL we have ORDER BY clause, which is always the last in fetching results from the database. To select all values for the table and sort them by column1 in descending order we write the next query:
```
SELECT *
FROM table1
ORDER BY column1 DESC
```
In pandas this same query will look like:
```
df.sort_values(by=['column1'], ascending=False)
```
___
### Credit
[Sergi Lehkyi](http://sergilehkyi.com/translating-sql-to-pandas/)  
