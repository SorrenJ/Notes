## Keywords Reference

https://www.w3schools.com/sql/sql_ref_keywords.asp


## SQL Data types

https://www.w3schools.com/sql/sql_datatypes.asp

## QUERYING DATA FROM A TABLE

```sql
SELECT c1, c2 FROM t;
```
Query data in columns c1, c2 from a table


```sql
SELECT * FROM t;
```
Query all rows and columns from a table


```sql
SELECT c1, c2 FROM t WHERE condition;
```
Query data and filter rows with a condition


```sql
SELECT DISTINCT c1 FROM t WHERE condition;
```
Query distinct rows from a table


```sql
SELECT c1, c2 FROM t ORDER BY c1 ASC [DESC];
```
Sort the result set in ascending or descending order


## QUERYING FROM MULTIPLE TABLES

```sql
SELECT c1, c2 FROM t1 INNER JOIN t2 ON condition;
```
Inner join t1 and t2

```sql
SELECT c1, c2 FROM t1 LEFT JOIN t2 ON condition;
```
Left join t1 and t1


```sql
SELECT c1, c2 FROM t1 RIGHT JOIN t2 ON condition;
```
Right join t1 and t2


```sql
SELECT c1, c2 FROM t1 FULL OUTER JOIN t2 ON condition;
```
Perform full outer join
