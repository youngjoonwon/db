### SQL Commands

##### nested queries, group by

#### a. upload database

use the same environment from the previous labs.

download ***library.sql*** from our course LMS.

```bash
$docker start docker-mysql
```

```shell
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./library.sql
$docker exec -it docker-mysql mysql -uroot -p
```

let's continue ...

#### b. nested queries

connect to the library database.

```sql
mysql> use library;
mysql> show tables;
```

show the followings. 

```
mysql> select id from books where publisher_id = 8;
mysql> select title from books where publisher_id = 8;
```

show all book titles published by 'Honford Star' (note that: \ sign is to indicate continuing line)

```sql
mysql> select title from books where publisher_id = \
       (select id from publishers where publisher = 'Honford Star');
```

or

```sql
mysql> select title from books where publisher_id in \
       (select id from publishers where publisher = 'Honford Star');
```

e.g.)

```sql
mysql> select title from books where publisher_id = \
    ->        (select id from publishers where publisher = 'Honford Star');
+----------------+
| title          |
+----------------+
| Ninth Building |
| Cursed Bunny   |
+----------------+
2 rows in set (0.00 sec)
```



show the total number of books published by 'Honford Star'.

```sql
mysql> select count(*) from books where publisher_id in \
       (select id from publishers where publisher = 'Honford Star');
```

e.g.)

```sql
mysql> select count(*) from books where publisher_id in \
    ->        (select id from publishers where publisher = 'Honford Star');
+----------+
| count(*) |
+----------+
|        2 |
+----------+
1 row in set (0.00 sec)
```

#### c. group by

show the number of books by each publisher.

```
mysql> desc books;
mysql> select count(id) from books group by publisher_id;
```

e.g.)

```
mysql> select count(id) from books group by publisher_id;
+-----------+
| count(id) |
+-----------+
|         3 |
|         3 |
|         2 |
|         1 |
|        12 |
...
```

show all translators' ids who translated more than one book.

```sql
mysql> desc translated;
mysql> select count(book_id), translator_id from translated group by translator_id having count(*) > 1;
```

e.g.)

```
mysql> select count(book_id), translator_id from translated group by translator_id having count(*) > 1;
+----------------+---------------+
| count(book_id) | translator_id |
+----------------+---------------+
|              2 |             8 |
|              2 |            16 |
|              3 |            29 |
|              2 |            35 |
|              2 |            40 |
|              3 |            48 |
|              2 |            52 |
|              2 |            62 |
|              4 |            68 |
+----------------+---------------+
9 rows in set (0.00 sec)
```



#### lab-db-04-question will be available soon. 

#### LMS online submission will be required. 
