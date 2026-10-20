### Environment setup 

#### a. Windows/Mac/Linux

run your docker desktop (download and install)

**For Mac: open terminal (Utilities -> Terminal) for Mac**

**For Windows: open mongosh from start menu, right click and choose administrator mode**



1. download mongodb docker image


```bash
$docker pull mongodb/mongodb-community-server:latest
```

output:

```shell
young-ultra] docker pull mongodb/mongodb-community-server:latest
latest: Pulling from mongodb/mongodb-community-server
4ce000a43472: Pull complete 
18514c2bc79a: Pull complete 
e709785a70e0: Pull complete 
efa2c3971fc8: Pull complete 
f45b55a04be7: Pull complete 
3181212a6141: Pull complete 
7190415e9175: Pull complete 
de8e085f5e82: Pull complete 
7b406eeb0deb: Pull complete 
4f4fb700ef54: Pull complete 
299b1e1b9dac: Pull complete 
Digest: sha256:cfc13c0d53f0d36997fb962393a91871bb86bd9ecf3e47e7f3ff070a25bdf905
Status: Downloaded newer image for mongodb/mongodb-community-server:latest
docker.io/mongodb/mongodb-community-server:latest
```

2. create mongodb docker

```bash
$docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest
$docker ps -a
```

output:

```shell
young-ultra] docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest

c4b91f8bbfa11e7f02a95ab5f64e162860d67bd47173b3fdd1d1699277a47599

young-ultra] docker ps -a

CONTAINER ID   IMAGE                                     COMMAND                  CREATED         STATUS                      PORTS                      NAMES
c4b91f8bbfa1   mongodb/mongodb-community-server:latest   "python3 /usr/local/…"   7 minutes ago   Up 7 minutes                0.0.0.0:27017->27017/tcp   mongodb
764ff17a7be6   mysql:latest                              "docker-entrypoint.s…"   12 days ago     Exited (0) 12 minutes ago                              docker-mysql
```

3. install mongosh

- instructions for windows/mac/linux:  https://www.mongodb.com/docs/mongodb-shell/install/

for mac, use home brew to install mongosh

```shell
$brew install mongosh
```

4. install mongosh

```shell
$mongosh --port 27017
```

output:

```shell
young-ultra] mongosh --port 27017
Current Mongosh Log ID:	66b9990dc9a52cf9c90f0a1f
Connecting to:		mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.2.15
Using MongoDB:		7.0.12
Using Mongosh:		2.2.15

For mongosh info see: https://docs.mongodb.com/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2024-08-12T05:02:46.194+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2024-08-12T05:02:46.809+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2024-08-12T05:02:46.810+00:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never' in this binary version
   2024-08-12T05:02:46.810+00:00: vm.max_map_count is too low
------

test> 
```

**You are almost done here.**



5. stop mongodb docker

```bash
$docker stop mongodb	
```

6. start mongodb docker

```bash
$docker start mongodb
```



