### Environment setup 

#### a. Windows/Mac/Linux

install docker desktop (download and install)

- https://docs.docker.com/guides/getting-started/get-docker-desktop/

- run your docker desktop


<br>

**For Mac: open terminal (Utilities -> Terminal) for Mac**

**For Windows: open powershell from start menu, right click and choose administrator mode**



1. download mysql docker image


```bash
$ docker pull mysql:latest
```

  or (if you want to run mysql version 8.0)

```bash
$ docker pull mysql:8.0
```

```
young-mbook/~] docker pull mysql:latest

latest: Pulling from library/mysql
c72f53f7235b: Pull complete 
c7e4ed755af2: Pull complete 
6c8c802f90bc: Pull complete 
eecc55f854cd: Pull complete 
cc8dabc09813: Pull complete 
e376e56dbcf9: Pull complete 
bd552de0e856: Downloading [===============================================>   ]   44.1MB/46.58MB
bd552de0e856: Pull complete 
78c8170ad3df: Pull complete 
4936147cb6bb: Pull complete 
9445ce8dad71: Pull complete 
Digest: sha256:d8df069848906979fd7511db00dc22efeb0a33a990d87c3c6d3fcdafd6fc6123
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview mysql:latest
```



2. create mysql docker

```bash
$ docker run --name docker-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
$ docker ps -a
```

```shell
young-mbook/~] docker run --name docker-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
868a42f2a5310844a50547d15a215cfccb40f59d12462976cb6b8d071e86c210

young-mbook/~] docker ps -a

CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                 NAMES
868a42f2a531   mysql:latest   "docker-entrypoint.s…"   12 seconds ago   Up 12 seconds   3306/tcp, 33060/tcp   docker-mysql
```



3. start mysql docker

```bash
$ docker exec -it docker-mysql mysql -uroot -p
```

```shell
young-mbook/~] docker exec -it docker-mysql mysql -uroot -p

Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 9.0.1 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> quit
Bye

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug docker-mysql
    Learn more at https://docs.docker.com/go/debug-cli/
```

**You are almost done here.**



4. stop mysql docker

```bash
$ docker stop docker-mysql	
```

5. start mysql docker

```bash
$ docker start docker-mysql
```



<br><br>

#### other useful commands:

```bash
$ docker image ls
```

```bash
$ docker rm docker-mysql
```

- import datafile to mysql


```bash
$ cat data.sql | docker exec -i docker-mysql mysql -uroot -ppassword db_name
```

  or

```bash
$ docker exec -i docker-mysql mysql -uroot -ppassword database < data.sql
```



