### Trying basics 

#### a. upload database

use the same environment from lab-db-01-mysql.

download ***book.sql*** from our course LMS.

```bash
$docker start docker-mysql
```

```shell
$docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./book.sql
$docker exec -it docker-mysql mysql -uroot -p
```

the following works in powershell v7.

```shell
$Get-Content ./book.sql | docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword'
```

<br>

the sample output should be similar like the followng: (password is '1234' here)

```
young-ultra 00-LAB-db] docker exec -i docker-mysql sh -c 'exec mysql -uroot -p1234' < ./book.sql
mysql: [Warning] Using a password on the command line interface can be insecure.

young-ultra 00-LAB-db] docker exec -it docker-mysql mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 36
Server version: 9.0.1 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

let's continue ...

#### b. simple example

1. SELECT every column

   ```sql
   mysql> use lab;
   mysql> select * from book;
   mysql> select * from book limit 5;
   ```

   e.g.)

   ```sql
   mysql> use lab;
   Reading table information for completion of table and column names
   You can turn off this feature to get a quicker startup with -A
   
   mysql> select * from book limit 5;
   +---------------+---------------------------------------+-------------------+-----------------------------+-----------+-------+-------------------------+------------+------+-------+--------+
   | isbn          | title                                 | author            | translator                  | format    | pages | publisher               | published  | year | votes | rating |
   +---------------+---------------------------------------+-------------------+-----------------------------+-----------+-------+-------------------------+------------+------+-------+--------+
   | 9788439736967 | Boulder                               | Eva Baltasar      | Nicole d'Amonville Alegría | paperback |   112 | Literatura Random House | 2022-08-02 | 2023 |  2779 |   3.77 |
   | 9781628971538 | Whale                                 | Cheon Myeong-Kwan | Jae Won Chung               | paperback |   368 | Europa Editions         | 2023-01-19 | 2023 |   175 |   3.97 |
   | 9781642861181 | The Gospel According to the New World | Maryse Condé     | Richard Philcox             | paperback |   184 | World Editions          | 2023-03-07 | 2023 |   114 |   3.05 |
   | 9781529414431 | Standing Heavy                        | Gauz              | Frank Wynne                 | paperback |   252 | MacLehose Press         | 2022-05-26 | 2023 |   322 |   3.57 |
   | 9781474623025 | Time Shelter                          | Georgi Gospodinov | Angela Rodel                | hardcover |   304 | W&N                     | 2022-04-21 | 2023 |  3142 |   4.05 |
   +---------------+---------------------------------------+-------------------+-----------------------------+-----------+-------+-------------------------+------------+------+-------+--------+
   5 rows in set (0.00 sec)
   
   ```

2. SELECT a few columns

   ```sql
   mysql> select title, author from book limit 1;
   ```

   e.g.)

   ```sql
   mysql> select title, author from book limit 1;
   +---------+--------------+
   | title   | author       |
   +---------+--------------+
   | Boulder | Eva Baltasar |
   +---------+--------------+
   1 row in set (0.00 sec)
   ```

3. SELECT more examples

   ```sql
   mysql> SELECT title, publisher FROM book WHERE year=2023;
   ```

   ```sql
   mysql> SELECT title FROM book WHERE title LIKE 'The%';
   ```

   ```sql
   mysql> SELECT title, year FROM book WHERE year = 2019 OR year = 2020;
   ```

   ```sql
   mysql> SELECT title, published FROM book WHERE published BETWEEN date('2023-01-01') AND date('2023-12-31');
   ```
   
   

#### c. create your sql file: put all your queries together

1. create an empty sql file

   ```shell
   $touch yourname-lab-sample.sql
   $vim yourname-lab-sample.sql
   ```

2. Insert the following and save

   ```sql
   use lab;
   /* query #1 */
   select title, author from book limit 1;
   /* query #2 */
   select title, publisher from book where year=2023 limit 1;
   ```
   
3. execute your queries in terminal

   ```shell
   $docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < yourname-lab-sample.sql
   ```

   for example

   ```shell
   $docker exec -i docker-mysql sh -c 'exec mysql -uroot -p1234' < young-lab-sample.sql
   ```

   e.g.)

   ```shell
   young-ultra 00-LAB-db] docker exec -i docker-mysql sh -c 'exec mysql -uroot -p1234' -sN < young-lab-sample.sql
   
   mysql: [Warning] Using a password on the command line interface can be insecure.
   title	author
   Boulder	Eva Baltasar
   title	publisher
   Boulder	Literatura Random House
   ```

#### **d. other useful commands**

```sql
mysql> show databases;
mysql> show tables;
mysql> desc table-name;
```



#### lab-db-02-question will be available soon. 

#### LMS online submission will be required.
