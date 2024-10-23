### Import CSV to MySQL

use the same environment from the previous labs (mysql docker instance).

download **data-music.csv** from our course LMS.

#### a. create database schema

first, let's take a look at **data-music.csv**. 

```shell
$ head -n 5 data-music.csv
```

e.g.)

```shell
young-ultra] head -n 5 data-music.csv
id,acousticness,danceability,duration_ms,energy,instrumentalness,mkey,mode,loudness,target,speechiness,tempo,time_signature,valence,target,song_title,artist
0,0.0102,0.833,204600,0.434,0.0219,2,0.165,-8.795,1,0.431,150.062,4.0,0.286,1,Mask Off,Future
1,0.199,0.743,326933,0.359,0.00611,1,0.137,-10.401,1,0.0794,160.083,4.0,0.588,1,Redbone,Childish Gambino
2,0.0344,0.838,185707,0.412,0.000234,2,0.159,-7.148,1,0.289,75.044,4.0,0.173,1,Xanny Family,Future
3,0.604,0.494,199413,0.338,0.51,5,0.0922,-15.236,1,0.0261,86.468,4.0,0.23,1,Master Of None,Beach House
```

some explanations are given here https://www.kaggle.com/datasets/geomack/spotifyclassification. However the column names may be slightly different. It will be fun to go through. 

let's create **music.sql**

```sh
$ vim music.sql
```

insert the following. (its schema can be different from the below).

```sql
DROP DATABASE IF EXISTS spotify;
CREATE DATABASE IF NOT EXISTS spotify;
USE spotify;

CREATE TABLE music ( 
	id INT NOT NULL,
	acousticness REAL NOT NULL,
	danceability REAL NOT NULL,
	duration_ms REAL NOT NULL,
	energy REAL NOT NULL,
	instrumentalness REAL NOT NULL,
	mkey REAL NOT NULL,
	liveness REAL NOT NULL,
	loudness REAL NOT NULL,
	mode REAL NOT NULL,
	speechiness REAL NOT NULL,
	tempo REAL NOT NULL,
	time_signature REAL NOT NULL,
	valence REAL NOT NULL,
	target INT NOT NULL,
	song_title VARCHAR(150) NOT NULL,
	artist VARCHAR(100) NOT NULL,
	PRIMARY KEY(id)
);
```

#### b. upload csv to table

before upload the csv file, we need to move this file to appropriate directory. 

our environment is at mysql-docker image, so we need to figure out where to put it. 

```shell
$ docker exec -it docker-mysql mysql -uroot -p
```

```sql
mysql> SELECT @@secure_file_priv;
```

e.g.)

```
mysql> SELECT @@secure_file_priv;
+-----------------------+
| @@secure_file_priv    |
+-----------------------+
| /var/lib/mysql-files/ |
+-----------------------+
1 row in set (0.00 sec)
```

The csv file should be under ''/var/lib/mysql-files/' in my environment (under docker). Let's quit mysql and come back to terminal. Copy data-music.csv for uploading.

```
$ docker cp data-music.csv docker-mysql:/var/lib/mysql-files/data-music.csv
```

add to the following at the end of music.sql.

```sql
LOAD DATA INFILE '/var/lib/mysql-files/data-music.csv' 
INTO TABLE music
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

we are almost done.

```
$ docker exec -i docker-mysql sh -c 'exec mysql -uroot -ppassword' < ./music.sql
```

done! now your database is ready to execute.

#### c. querying 

again, enter mysql.

```
$ docker exec -it docker-mysql mysql -uroot -p
```

```
mysql> use spotify;
mysql> show tables;
mysql> desc music;
mysql> select * from music limit 5;
```

e.g.)

```
mysql> select * from music limit 5;
+----+--------------+--------------+-------------+--------+------------------+------+----------+----------+------+-------------+---------+----------------+---------+--------+----------------+------------------+
| id | acousticness | danceability | duration_ms | energy | instrumentalness | mkey | liveness | loudness | mode | speechiness | tempo   | time_signature | valence | target | song_title     | artist           |
+----+--------------+--------------+-------------+--------+------------------+------+----------+----------+------+-------------+---------+----------------+---------+--------+----------------+------------------+
|  0 |       0.0102 |        0.833 |      204600 |  0.434 |           0.0219 |    2 |    0.165 |   -8.795 |    1 |       0.431 | 150.062 |              4 |   0.286 |      1 | Mask Off       | Future           |
|  1 |        0.199 |        0.743 |      326933 |  0.359 |          0.00611 |    1 |    0.137 |  -10.401 |    1 |      0.0794 | 160.083 |              4 |   0.588 |      1 | Redbone        | Childish Gambino |
|  2 |       0.0344 |        0.838 |      185707 |  0.412 |         0.000234 |    2 |    0.159 |   -7.148 |    1 |       0.289 |  75.044 |              4 |   0.173 |      1 | Xanny Family   | Future           |
|  3 |        0.604 |        0.494 |      199413 |  0.338 |             0.51 |    5 |   0.0922 |  -15.236 |    1 |      0.0261 |  86.468 |              4 |    0.23 |      1 | Master Of None | Beach House      |
|  4 |         0.18 |        0.678 |      392893 |  0.561 |            0.512 |    5 |    0.439 |  -11.648 |    0 |      0.0694 | 174.004 |              4 |   0.904 |      1 | Parallel Lines | Junior Boys      |
+----+--------------+--------------+-------------+--------+------------------+------+----------+----------+------+-------------+---------+----------------+---------+--------+----------------+------------------+
5 rows in set (0.00 sec)
```

play around!

#### d. sample questions

- Show all the songs where tempo is greater than 150 (inclusive), and count them. 
- Delete the songs where tempo is less than eqaul to 10. 
- anything else? 