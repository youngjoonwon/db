### Playing with InfluxDB (timeseries database).

#### (Questions at the bottom of this page)

##### a. Install InfluxDB container in docker environment.

0. Make sure that your docker is running. InfluxDB: https://www.influxdata.com/ 

1. If you are running on mac or linux, then do the following:

   ```bash
   $curl -O https://www.influxdata.com/d/install_influxdb3.sh \
   && sh install_influxdb3.sh enterprise
   ```

   If Windows, pull the influxdb:3 docker image.

   ```bash
   $docker pull influxdb:3-enterprise
   ```

2. If mac or linux, you may see the following: Enter choice 1.

   ```bash
   ┌───────────────────────────────────────────────────┐
   │ Welcome to InfluxDB! We'll make this quick.       │
   └───────────────────────────────────────────────────┘
   
   Select Installation Type
   
   1) Docker Image    (The official Docker image)
   2) Simple Download (No dependencies required)
   
   Enter your choice (1-2): 1
   
   Download and Tag Docker Image
   ├─ docker pull influxdb:enterprise
   └─ docker tag influxdb:enterprise influxdb3-enterprise
   
   3-enterprise: Pulling from library/influxdb
   97dd3f0ce510: Pull complete 
   1f4ba7d458e8: Pull complete 
   ba2d18cf293d: Pull complete 
   63e2bca99991: Pull complete 
   fd89ddcab6d4: Pull complete 
   8b5ded256462: Pull complete 
   Digest: sha256:71bf9f4ecabca602da288c3e76a429b4327a41c97a36f76a00c627747c0a13cd
   Status: Downloaded newer image for influxdb:3-enterprise
   docker.io/library/influxdb:3-enterprise
   ...
   ```

3. Run docker image. And select HOME USE (option 3). Enter your email and validate your account.

   ```bash
   $docker run -it -p 8181:8181 --name influxdb3-container --volume ~/.influxdb3_data:/.data --volume ~/.influxdb3_plugins:/plugins influxdb:3-enterprise influxdb3 serve --cluster-id c0 --node-id node0 --object-store file --data-dir /.data --plugin-dir /plugins 
   
   Catalog initialized with uuid: 'f48c0757-36b4-451c-bccf-d0b0c1c076bb' and storage hash: 'sha256:fW5z7-f4BAqNBQ6YSMm6i2d17BY2122r0RCjD4zCpnM'
   Didn't find license in object store: Access of Object Store failed: Object at location /.data/c0/trial_or_home_license not found: No such file or directory (os error 2)
   No valid license found: Requested license not available.
   InfluxDB 3 Enterprise Setup
   
   To get started, please select a license type:
   
   1) FREE TRIAL
      └─ Full features for 30 days (up to 256 cores), perfect for evaluating all Enterprise capabilities
   
   2) COMMERCIAL
      └─ Paid commercial license, enabling flexible production environments with dedicated support
   
   3) HOME USE
      └─ Free for non-commercial use. Max 2 cores and single node only, ideal for hobbyists and personal projects
   
   Enter option [1-3]: 
   3
   Please enter your email for a Home license: 
   id@example.com
   Email sent to id@example.com. Please check your inbox to verify your email address and proceed.
   Waiting for verification...
   
   ... INFO influxdb3_lib::commands::serve: Creating io runtime executor w/ 1 threads
   ...
   ```

4. Create Token to access your InfluxDB. And save it somewhere. It only appears once. 

   ```bash
   $docker exec -it influxdb3-container influxdb3 create token --admin
   
   New token created successfully!
   
   Token: apiv3_Put4nbOZOvO8qeUkpYI8P-gISDPT3TVWQsDxYoo8jR5pTUM94MbpGzrINoA9umZ4ZXLjiR7v8AaB5_udJnyukg
   HTTP Requests Header: Authorization: Bearer apiv3_Put4nbOZOvO8qeUkpYI8P-gISDPT3TVWQsDxYoo8jR5pTUM94MbpGzrINoA9umZ4ZXLjiR7v8AaB5_udJnyukg
   
   IMPORTANT: Store this token securely, as it will not be shown again.
   ```

5. Create a new database called, coin. 

   A database retention period is the maximum age of data stored in the database. The minimum practical retention period is 1 hour. 

   ```bash
   $docker exec -it influxdb3-container influxdb3 create database --retention-period 1d coin --token TOKEN
   ```

   [Reference] https://docs.influxdata.com/influxdb3/enterprise/admin/databases/

6. Verify your database.

   ``` bash
   $docker exec -it influxdb3-container influxdb3 show databases --token TOKEN
   ```

7. If you want to try other queries, here's an example.

   ```bash
   $docker exec -it influxdb3-container influxdb3 query --database coin "SHOW tables" --token TOKEN
   ```

   Now you are ready to continue. 


#### b. Query InfluxDB

There are several ways to interact with InfluxDB. Let's take a look at ODBC example. However, do NOT use ODBC approach to complete your questions below.
[Reference] https://docs.influxdata.com/influxdb3/enterprise/query-data/execute-queries/odbc/

```python
import pyodbc

# Connect to InfluxDB
conn = pyodbc.connect(
    'DSN=InfluxDB3',
    autocommit=True
)

# Create cursor
cursor = conn.cursor()

# Execute query
cursor.execute("""
    SELECT
        time,
        temp,
        location
    FROM
        home
    WHERE
        time >= now() - INTERVAL '1 hour'
    ORDER BY
        time DESC
""")

# Fetch results
for row in cursor.fetchall():
    print(row)

# Close connection
cursor.close()
conn.close()
```

Now, download 'influxdb.zip' from our course LMS. 

#### Question 1.

Run fetch-and-write.py:

- In order to run it, you should complete influxdb_config.py first and do not modify the code in fetch-and-write.py. 
- Question 1-1. Show its running screen. Provide a screenshot of your terminal. 
- Question 1-2. Show its query and result: Show the first 5 lines of the table in terminal (e.g., Step 7 above). Provide a screenshot of your terminal.
- Question 1-3. Explain what this program is doing in your own words. A couple sentences long should be just fine. Do not explain the code itself. 
- Put 1-1, 1-2, and 1-3 into a single PDF file and submit to LMS, PDF file only (db-lab-13week-question-1-submission)

#### Question 2. 

Using the template file, influx-query.py:

- Complete this python client program to query the database created (possibly running at the same time) from Question 1. Here's a question: "Show all rows of sellprice, buyprice, and its time in the last 20 seconds. And these rows should be sorted in time, the most recent data first." It should run like the following:

  ```bash
  $python ./influx-query.py
        sellprice     buyprice                          time
  0   142590000.0  142571000.0 2025-11-17 10:10:59.925813214
  1   142590000.0  142560000.0 2025-11-17 10:10:53.910075711
  2   142571000.0  142560000.0 2025-11-17 10:10:47.901999167
  3   142582000.0  142560000.0 2025-11-17 10:10:41.936012678
  ...
  ```

  Again, do not use ODBC or JDBC. You must use python **InfluxDBClient3**. 

  [Reference] https://github.com/InfluxCommunity/influxdb3-python

- Submit your influx-query.py to LMS, .py file only (db-lab-13week-question-2-submission)

- DO NOT put any space in your submit file name.

- If your program shows any errors, 0 will be given.
