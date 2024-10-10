### SQL Commands

##### set, join

#### a. upload database

use the same environment from the previous labs.

again, use ***library.sql*** from our course LMS.

```bash
$docker start docker-mysql
```

```shell
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./library.sql
$docker exec -it docker-mysql mysql -uroot -p
```

let's continue ...

#### b. set

connect to the library database.

```sql
mysql> use library;
mysql> show tables;
```

put together all authors and translators

```sql
mysql> select name from authors UNION select name from translators;
```

put together all authors (id, name) and translators (id, name)

```sql
mysql> select id, name from authors UNION select id, name from translators;
```

show translators, but not authors

````sql
mysql> select name from authors INTERSECT select name from translators;
````

e.g.)

````sql
mysql> select name from authors INTERSECT select name from translators;
+---------------------+
| name                |
+---------------------+
| Ngũgĩ wa Thiong'o |
+---------------------+
1 row in set (0.00 sec)
````

#### c. upload database

again, use ***book.sql*** from our course LMS.

```bash
$docker start docker-mysql
```

let's add an extra table.

```shell
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./book.sql
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./teams.sql
$docker exec -it docker-mysql mysql -uroot -p
```

let's continue ...

#### d. join 

show all the teams having travel records.

```sql
mysql> select * from teams JOIN travel ON travel.id = teams.id;
```

show all the teams in left join?

```sql
mysql> select * from teams LEFT JOIN travel ON travel.id = teams.id;
```

show all the teams in natural join? (that are matching without duplicates?)

```sql
select * from teams NATURAL JOIN travel;
```

e.g.)

```sql
mysql> select * from teams natural join travel;
+-----+--------+----------+----------+------+
| id  | name   | location | distance | days |
+-----+--------+----------+----------+------+
| 127 | twins  | seoul    |      100 |   10 |
| 128 | eagles | deajeon  |      153 |    5 |
| 129 | giants | busan    |      137 |    3 |
| 132 | lions  | deagu    |      162 |   62 |
| 134 | tigers | gwangju  |      191 |   58 |
+-----+--------+----------+----------+------+
5 rows in set (0.00 sec)
```

#### f. sample questions

- Query #1: Can you create the same join query using netsted or set queires?
- Query #2: Can you join multiple tables (more than two) to have a proper query?
- Query #3: Can you add an extra condition in the join statement?
- Try it on your own for fun.
