### SQL Commands

##### order by, aggregate 

#### a. upload database

use the same environment from the previous labs.

again, use ***book.sql*** from our course LMS.

```bash
$docker start docker-mysql
```

```shell
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./book.sql
$docker exec -it docker-mysql mysql -uroot -p
```

let's continue ...

#### b. order by

show some book info by its rating.

```sql
mysql> use lab;
mysql> select * from book;
mysql> select title, author from book order by rating limit 5;
```

e.g.)

```sql
mysql> select title, author FROM book ORDER BY rating LIMIT 5;
+---------------------------------------+------------------+
| title                                 | author           |
+---------------------------------------+------------------+
| The Gospel According to the New World | Maryse Condé    |
| The Pine Islands                      | Marion Poschmann |
| Love in the New Millennium            | Can Xue          |
| After the Sun                         | Jonas Eika       |
| I Live in the Slums                   | Can Xue          |
+---------------------------------------+------------------+
5 rows in set (0.01 sec)
```

what's desc here?

```sql
mysql> select title, author from book order by rating desc limit 5; 
```

#### c. aggregate

show the total number of books.

```sql
mysql> select count(*) from book;
```

show the max rating. 

```sql
mysql> select max(rating) from book;
```

its opposite

```sql
mysql> select min(rating) from book;
```

#### d. create your sql file: put all your queries together

use the same instruction from section c. in lab-db-02. 

#### e. sample questions

- Query #1: What's the total number of translators in this book database?
- Query #2: What's the total number of unique translators?
- Query #3: What's the average number of pages?
- Query #4: Show the bottom 5 books by ratings (display in increasing order of ratings)
- Try it on your own for fun.

#### Let's try a few more in class.
