### Playing with Elasticsearch.

#### (Questions at the bottom of this page)

##### a. Install Elasticsearch container in docker environment. 

0. You do not have to use docker for installation, you can try on your local machine directly. The following instructions are for docker. Elasticsearch: https://www.elastic.co/

1. Make sure that your docker is running. 

2. Create new docker network and pull the Elasticsearch docker image (version 9.2.1).

   ```bash
   $docker network create elastic
   $docker pull docker.elastic.co/elasticsearch/elasticsearch:9.2.1
   ```

3. Start the container.

   ```bash
   $docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:9.2.1
   ```

   You may encounter something like this:

   ```
   ...
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   ✅ Elasticsearch security features have been automatically configured!
   ✅ Authentication is enabled and cluster connections are encrypted.
   
   ℹ️  Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
     xZs-QjoTo3i-gQF2t7LZ
   
   ℹ️  HTTP CA certificate SHA-256 fingerprint:
     d035d58062d0b07560af4637eb059e11d2b61ea7efcd40a3109581554ac84098
   
   ℹ️  Configure Kibana to use this cluster:
   • Run Kibana and click the configuration link in the terminal when Kibana starts.
   • Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
     eyJ2ZXIiOiI4LjE0LjAiLCJhZHIiOlsiMTcyLjE4LjAuMjo5MjAwIl0sImZnciI6ImQwMzVkNTgwNjJkMGIwNzU2MGFmNDYzN2ViMDU5ZTExZDJiNjFlYTdlZmNkNDBhMzEwOTU4MTU1NGFjODQwOTgiLCJrZXkiOiIyQ1VUZUpvQnl3bXVJVFVYVjVtbzpxV21NRjNaTXJ6LWZKYmFkcWZxbElnIn0=
   
   ℹ️ Configure other nodes to join this cluster:
   • Copy the following enrollment token and start new Elasticsearch nodes with `bin/elasticsearch --enrollment-token <token>` (valid for the next 30 minutes):
     eyJ2ZXIiOiI4LjE0LjAiLCJhZHIiOlsiMTcyLjE4LjAuMjo5MjAwIl0sImZnciI6ImQwMzVkNTgwNjJkMGIwNzU2MGFmNDYzN2ViMDU5ZTExZDJiNjFlYTdlZmNkNDBhMzEwOTU4MTU1NGFjODQwOTgiLCJrZXkiOiIyaVVUZUpvQnl3bXVJVFVYVjVtbzo3LWdvcVkxWGRoUGhwR2s2aC1qVFlBIn0=
   
     If you're running in Docker, copy the enrollment token and run:
     `docker run -e "ENROLLMENT_TOKEN=<token>" docker.elastic.co/elasticsearch/elasticsearch:9.2.1`
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   ...
   ```

4. Copy the generated elastic password and enrollment token above. These only appear once at the first installation. If you want to regenerate the credential, do the following:

   ```bash
   $docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
   ```

5. Open a new shell (terminal) and set password to your environment. 

   ```bash
   $export ELASTIC_PASSWORD="xZs-QjoTo3i-gQF2t7LZ"
   ```

6. Restart your shell.

7. Get (copy) the certificate from the container to your local machine.

   ``` bash
   $docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
   ```

8. Make a REST API call to Elasticsearch to ensure the container is running (to see everything is okay). 

   ```bash
   $curl --cacert http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200
   
   {
     "name" : "1ab4c7f53873",
     "cluster_name" : "docker-cluster",
     "cluster_uuid" : "snH2CT1STLKGHDEL4D5-eg",
     "version" : {
       "number" : "9.2.1",
       "build_flavor" : "default",
       "build_type" : "docker",
       "build_hash" : "4ad0ef0e98a2e72fafbd79a19fa5cae2f026117d",
       "build_date" : "2025-11-06T22:07:39.673130621Z",
       "build_snapshot" : false,
       "lucene_version" : "10.3.1",
       "minimum_wire_compatibility_version" : "8.19.0",
       "minimum_index_compatibility_version" : "8.0.0"
     },
     "tagline" : "You Know, for Search"
   }
   ```

   Now you are ready!  [Reference] https://www.elastic.co/docs/deploy-manage/deploy/self-managed/install-elasticsearch-with-docker

9. (This step is not necessary) However, If you are having trouble with the above installation, you can try an automated installation script at the beginning: NOT RECOMMENDED for our lab.

   ```bash
   $curl -fsSL https://elastic.co/start-local | sh
   ```

   The script generates a random password for the `elastic` user, which is displayed at the end of the installation and stored in the `.env` file.

10. Pull and start Kibana (visualization and exploration tool for Elasticsearch)

    ```bash
    $docker pull docker.elastic.co/kibana/kibana:9.2.1
    
    $docker run --name kib01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:9.2.1
    
    ...
    i Kibana has not been configured.
    
    Go to http://0.0.0.0:5601/?code=024202 to get started.
    ...
    ```

11. Open the link above e.g.) "http://0.0.0.0:5601/?code=024202" and enter the enrollment token that was generated from Step 3 (valid for 30 minutes). 

12. If you want to regenerate the credential, do the following and use it:

    ```bash
    $docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
    ```

13. Log in Kibana as elastic user with password again from Step 3. 

    Now you are really done! Play with it! 
    [Reference] https://www.elastic.co/docs/explore-analyze

14. Other helpful commands:

    ```bash
    # Remove the Elastic network
    $docker network rm elastic
    
    # Remove Elasticsearch containers
    $docker rm es01
    
    # Remove the Kibana container
    $docker rm kib01
    ```

    

#### b. Sending requests to Elasticsearch

There are several ways to interact with Elasticsearch. 
[Reference] https://github.com/elastic/elasticsearch

1. Kibana console, Management->Dev Tools

2. Using curl (e.g., shell)

   ```bash
   curl -u elastic:$ELASTIC_PASSWORD \
     -X PUT \
     https://localhost:9200/my-new-index \
     -H 'Content-Type: application/json'
   ```

3. Program client template (e.g., python client)

    ```python
    # Python client template for Question 2.
    # Elasticsearch endpoint: http://localhost:9200
    # username: elastic
    # password: 'ELASTIC_PASSWORD' from Step 3 in Section a.
    
    import os
    from elasticsearch import Elasticsearch
    
    # Do not change anything ============= start
    username = 'elastic'
    password = os.getenv('ELASTIC_PASSWORD') # Value you set in the environment variable
    
    client = Elasticsearch(
        "https://localhost:9200",
        basic_auth=(username, password)
    )
    print(client.info())
    # ============= end
    
    # Add your code from here for Question 2.
    
    ```

   Please refer to https://www.elastic.co/docs/reference/elasticsearch/clients/python for more examples. Go ahead with LLM agents that you like for more. 

Now, download 'elasticsearch-demo-michelin.txt' and 'michelin-list.txt' from our course LMS. Complete/Modify/Execute steps in 'elasticsearch-demo-michelin.txt' to answer.

#### Question 1.

Using Kibana, Management->Dev Tools, complete the following:

- Question 1-1. Show its request and results: All the restaurants in California that are Mexican. Provide a screenshot of your Kibana working. 
- Question 1-2. Show its requests and results: Among all the restaurants in California that are Contemporary, show only the first three of restaurant names and its star (from the highest). Provide a screenshot of your Kibana working (again, no need to show all restaurants in the screenshot).
- Question 1-3. Show its request and result: The restaurant that is the cheapest and closest to lat: 39.802765 and lon: -105.087486. Provide a screenshot of your Kibana working.
- Put 1-1, 1-2, and 1-3 into a single PDF file and submit to LMS, PDF file only (db-lab-12week-question-1-submission)

#### Question 2. 

Using the python client template (or similar) above,

- Write a python client program (e.g., elastic-search.py) to show the same result of Question 1-1. Submit your code only.

  [Reference] https://github.com/elastic/elasticsearch-py

- Submit it to LMS, .py file only (db-lab-12week-question-2-submission)

- DO NOT put any space in your submit file name.

- If your program shows any errors, 0 will be given.
