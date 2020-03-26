# Introduction

Convert SQL Queries into Pandas

## SELECT

### SQL
```
# Select everything from table1
SELECT *
FROM table1
```
### Pandas
```
df.head()
```
### SQL
```
SELECT column1, column2
FROM table1
```
### Pandas
```
df[['column1', 'column2']]
```

## WHERE

### SQL
```
SELECT column1, column2
FROM table1
WHERE column1 = 100
```
### Pandas
```
df[['column1', 'column2']].loc[df['column1'] == 100]
```
### SQL
```
SELECT *
FROM table1
WHERE column1 > 10 AND column2 < 100
```
### Pandas
```
df.loc[(df['column1'] > 10) & (df['column2'] < 100)]
```
### Pandas
```
df.loc[(df['column1'] > 10) | ~(df['column2'] < 100)]
```
### SQL
```
SELECT *
FROM table1
WHERE column1 BETWEEN 1 and 3 AND column2 IN (10,20) AND column3 LIKE '%ewyor%'
```
### Pandas
```
df.loc[(df['colum1'].between(1,3)) & (df['column2'].isin([10,20])) & (df['column3'].str.contains('ewyor'))]
```

## JOIN

### SQL
```
SELECT t1.column1, t2.column1
FROM table1 t1
INNER JOIN table2 t2 ON t1.column_id = t2.column_id
```
### Pandas
```
df_joined = df1.join(df2, on='column_id', how='inner')
df_joined.loc[['column1_df1', 'column1_df2']]
```

## GROUP BY

### SQL
```
SELECT column1, count(*)
FROM table1
GROUP BY column1
```
### Pandas
```
df.groupby('column1')['column1'].count()
```
### SQL
```
SELECT store, sum(employees)
FROM table1
GROUP BY branch
HAVING sum(employees) > 10
```
### Pandas
```
df_grouped = df.groupby('branch')['employees'].sum()
df_grouped.loc[df_grouped > 100]
```
## ORDER BY

### SQL
```
SELECT *
FROM table1
ORDER BY column1 DESC
```
### Pandas
```
df.sort_values(by=['column1'], ascending=False)
```
