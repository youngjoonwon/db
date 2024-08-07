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
$ docker pull mysql:lastest
```

  or (if you want to run mysql version 8.0)

```bash
$ docker pull mysql:8.0
```



2. create mysql docker

```bash
$ docker run --name docker-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
$ docker ps -a
```

3. start mysql docker

```bash
$ docker exec -it docker-mysql mysql -uroot -p
```

4. stop mysql docker

```bash
$ docker stop docker-mysql	
```

5. start mysql docker

```bash
$ docker start docker-mysql
```



#### other useful commands:

```bash
$ docker image ls
```

```bash
$ docker rm docker-mysql
```

- import datafile (csv) to mysql


```bash
$ cat data.sql | docker exec -i docker-mysql mysql -uroot -ppassword db_name
```

  or

```bash
$ docker exec -i docker-mysql mysql -uroot -ppassword database < data.sql
```



