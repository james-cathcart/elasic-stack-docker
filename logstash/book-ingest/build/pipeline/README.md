# Ingest Pipeline for Book Examples

This pipeline is intended as an **ingestion pipeline** for example CSV-based data in order to follow along with the book: Learning Elastic Stack 7.0 by Pranav Shukla and Sharath Kumar M N

## Ingest CSV File into ElasticSearch via Docker & Logstash

The idea here is to ingest CSV file contents into ElasticSearch without having to install Logstash on your local machine. Instead, we will use a two part solution:
 - A Dockerfile defining a custom Logstash image with a Volume mapping for our data file.
 - A docker-compose.yml file for quickly and easily configuring and running the Logstash container.

 ### Step 1) Modify the pipeline configuration
 In the `pipeline/logstash.conf` file, replace the `<your-file>` text with your filename in the `input` section:
 ```
 path => "/data/<your-file>.csv"
 ```

 In the `output` section, replace the `<elastic_search_host>` text with your ElasticSearch host IP:
 ```
 hosts => "http://<elastic_search_host>:9200"
 ```

  ### Step 2) Update docker-compose.yml
  Under the `volumes` section, replace the `<host-path>` text with the path to the directory that contains your CSV file:
  ```
  volumes:
    - "/your/data/dir:/data"
  ```

  ### Step 3) Build and run docker-compose.yml
  Run the following commands in the `book-ingest/` directory:
  ```
  docker-compose build
  docker-compose up -d
  docker logs logstash-ingest --follow
  ```
  The container will start, Logstash will load the pipeline configuration, and the CSV file will be parsed and sent to ElasticSearch.

  When the operation has completed, press `^C` to exit the logs and run the command:
  ```
  docker-compose down
  ```
  This will destroy the container. You may optionally choose to also delete the Logstash Docker image that was created during this process.
