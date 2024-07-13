[[SQL Dictionary]]
## Setup for WSL2

### Start 

`sudo service postgresql start'

OR

``` bash
startpostgres
```


### Stop

`sudo service postgresql stop`



### Connect to PostgreSQL

`psql`

Creating a super user

``` sql
CREATE ROLE development LOGIN SUPERUSER PASSWORD 'development';
```

Create the database

``` sql
CREATE DATABASE development;
```

Exit the PostgreSQL console
`/q`



### Create a Test Database

``` sql
CREATE DATABASE test_db;
```

Connect to the database

``` sql
\c test_db;
```

Output

```terminal
psql (9.2.4)
Type "help" for help.
You are now connected to database "testdb" as user "postgres".
test_db=#
```

### Create a Sample Table

creating a table for `famous_people`

```sql
CREATE TABLE famous_people (
  id SERIAL PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  birthdate DATE
);
```


#### INSERT data

```sql
INSERT INTO famous_people (first_name, last_name, birthdate)
  VALUES ('Abraham', 'Lincoln', '1809-02-12');
INSERT INTO famous_people (first_name, last_name, birthdate)
  VALUES ('Mahatma', 'Gandhi', '1869-10-02');
INSERT INTO famous_people (first_name, last_name, birthdate)
  VALUES ('Paul', 'Rudd', '1969-04-06');
INSERT INTO famous_people (first_name, last_name, birthdate)
  VALUES ('Paul', 'Giamatti', '1967-06-06');
```


#### SELECT data

Select all columns for all rows in the `famous_people` table.

```sql
SELECT * FROM famous_people; -- should give us back all 4
```

Output

```
 id | first_name | last_name | birthdate
----+------------+-----------+------------
  1 | Abraham    | Lincoln   | 1809-02-12
  2 | Mahatma    | Gandhi    | 1869-10-02
  3 | Paul       | Rudd      | 1969-04-06
  4 | Paul       | Giamatti  | 1967-06-06
(4 rows)
```


Only return people with a birthday on or after `'1920-01-01'`.

```sql
SELECT * FROM famous_people WHERE birthdate >= '1920-01-01';
```

Output

```
 id | first_name | last_name | birthdate
----+------------+-----------+------------
  3 | Paul       | Rudd      | 1969-04-06
  4 | Paul       | Giamatti  | 1967-06-06
(2 rows)
```

  
Only return people with a birthday before `'1920-01-01'`.

```sql
SELECT * FROM famous_people WHERE birthdate < '1920-01-01';
```

Output

```
 id | first_name | last_name | birthdate
----+------------+-----------+------------
  1 | Abraham    | Lincoln   | 1809-02-12
  2 | Mahatma    | Gandhi    | 1869-10-02
(2 rows)

```

Only return people with the first name `'Paul'`.

```sql
SELECT * FROM famous_people WHERE first_name = 'Paul';
```

Output

```
 id | first_name | last_name | birthdate
----+------------+-----------+------------
  3 | Paul       | Rudd      | 1969-04-06
  4 | Paul       | Giamatti  | 1967-06-06
(2 rows)
```

Select the total number of people in the `famous_people` table.

```sql
SELECT count(*) FROM famous_people;
```


Output

```
count
-------
     4
(1 row)
```


## Adding a newly created table to your database

Your sql file

```sql
CREATE TABLE cohorts (
  id SERIAL PRIMARY KEY NOT NULL,
  name VARCHAR(255) NOT NULL,
  start_date DATE,
  end_date DATE
);

CREATE TABLE students (
  id SERIAL PRIMARY KEY NOT NULL,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  phone VARCHAR(32),
  github VARCHAR(255),
  start_date DATE,
  end_date DATE,
  cohort_id INTEGER REFERENCES cohorts(id) ON DELETE CASCADE
);
```

Lets add these 2 tables  to our database

```shell
\i migrations/students_cohorts.sql
```

check if the 2 tables have been created

```sh
\dt
```

output 

``` shell
         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+--------
 public | cohorts  | table | labber
 public | students | table | labber
(2 rows)
```


## Downloading sqls files

in your terminal

```shell
wget https://bit.ly/2Z0fN4t -O seeds/students.sql

wget https://bit.ly/300nIQy -O seeds/cohorts.sql
```

## Query examples

Tables

students.sql

```sql
INSERT INTO students (id, name, email, phone, github, start_date, end_date, cohort_id) VALUES (1, 'Armand Hilll', 'lera_hahn@dickens.org', '778-349-3299', 'aspernatur', '2018-02-12T08:00:00.000Z', '2018-04-20T07:00:00.000Z', 1);
```


cohorts.sql

``` sql
INSERT INTO cohorts (id, name, start_date, end_date) VALUES (1, 'FEB12', '2018-02-12T08:00:00.000Z', '2018-04-20T07:00:00.000Z');
```

### Query 1

Get the names of all of the students from a single cohort.

```sql
SELECT id, name 
FROM students 
WHERE cohort_id = 1
ORDER BY name;
```

output

``` shell
id |       name        
----+-------------------
  1 | Armand Hilll
 13 | Brian Jones
 16 | Carmel Grant
 14 | Clint Cremin
 17 | Colten Gutkowski
...
(18 rows)


```


### Query 2

Select the total number of students who were in the first 3 cohorts.

```sql
SELECT count(id)
FROM students 
WHERE cohort_id IN (1,2,3);
```

Output

```shell
 count 
-------
    48
(1 row)
```

### Query 3

Get all of the students that don't have an email or phone number. - Get their `name`, `id`, and `cohort_id`.

```sql
SELECT name, id, cohort_id
FROM students
WHERE email IS NULL
OR phone IS NULL;
```

output

```shell
       name       | id  
------------------+-----
 Aurore Yundt     | 160
 Cory Toy         | 161
 Kurt Turcotte    | 163
 Elda McClure     | 164
 Luisa Sipes      | 168
...
(17 rows)
```


### Query 4

Get all of the students without a `gmail.com` account and a phone number. - Get their `name`, `email`, `id`, and `cohort_id`.

```sql
SELECT name, id, email, cohort_id
FROM students
WHERE email NOT LIKE '%gmail.com'
AND phone IS NULL;
```

output

```shell
      name       | id  |           email           | cohort_id 
-----------------+-----+---------------------------+-----------
 Javonte Ward    | 178 | jessie_howell@hotmail.com |        12
 Jessika Jenkins | 187 | stephanie.koss@kevon.io   |        12
 Jerrold Rohan   | 189 | wehner.twila@hotmail.com  |        12
(3 rows)
```


### Query 5

Get all of the students currently enrolled. Get their `name`, `id`, and `cohort_id`. Order them by `cohort_id`. A student's end date will be `NULL` when they are currently enrolled in Bootcamp.

```sql
SELECT name, id, cohort_id
FROM students
WHERE end_date IS NULL
ORDER BY cohort_id;
```

output

```shell
        name         | id  | cohort_id 
---------------------+-----+-----------
 Deon Hahn           | 151 |        11
 Sean Bartell        | 152 |        11
 Sarai Flatley       | 153 |        11
 Billie Mitchell     | 154 |        11
 Vance Kihn          | 155 |        11
...
(42 rows)
```
### Query 6

Get all graduates without a linked Github account. - Get their `name`, `email`, and `phone`.

```sql
SELECT name, email, phone
FROM students
WHERE github IS NULL
AND end_date IS NOT NULL;
```

output

```shell
       name        |             email             |    phone     
-------------------+-------------------------------+--------------
 Herminia Smitham  | sawayn.sarina@yahoo.com       | 778-251-5094
 Jacinthe Kautzer  | litzy_fay@hilpert.net         | 075-883-5570
 Bernardo Turcotte | margarita.anderson@paolo.name | 814-473-6929
 Eloisa Quigley    | schmidt.ansel@gmail.com       | 276-965-2022
 Tiana Altenwerth  | zelda.stanton@yahoo.com       | 448-872-0954
 Hailie Kutch      | zora.corkery@goldner.net      | 249-763-9998
(6 rows)
```