### Setting up an Elasticsearch Cluster with Docker Compose

This detailed guide will walk you through the step-by-step process of setting up an Elasticsearch cluster using Docker Compose. We'll cover each aspect, from creating the Docker Compose file to checking the cluster status.

#### Prerequisites:
- Docker installed on your system.

#### Step 1: Create a Docker Compose file
Create a file named `docker-compose.yml` and copy the following content into it:

```yaml
version: '3.2'
services:
  melkradar-adver-cluster-live-adver-0:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.0
    container_name: melkradar-adver-cluster-live-adver-0
    environment:
      - node.name=melkradar-adver-cluster-live-adver-0
      - cluster.name=melkradar-adver-cluster-live-adver
      - discovery.seed_hosts=melkradar-adver-cluster-live-adver-1,melkradar-adver-cluster-live-adver-2
      - cluster.initial_master_nodes=melkradar-adver-cluster-live-adver-0,melkradar-adver-cluster-live-adver-1,melkradar-adver-cluster-live-adver-2
      - bootstrap.memory_lock=true
      - xpack.security.enabled=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "xpack.security.transport.ssl.enabled=true"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic

  melkradar-adver-cluster-live-adver-1:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.0
    container_name: melkradar-adver-cluster-live-adver-1
    environment:
      - node.name=melkradar-adver-cluster-live-adver-1
      - cluster.name=melkradar-adver-cluster-live-adver
      - discovery.seed_hosts=melkradar-adver-cluster-live-adver-0,melkradar-adver-cluster-live-adver-2
      - cluster.initial_master_nodes=melkradar-adver-cluster-live-adver-0,melkradar-adver-cluster-live-adver-1,melkradar-adver-cluster-live-adver-2
      - bootstrap.memory_lock=true
      - xpack.security.enabled=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "xpack.security.transport.ssl.enabled=true"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data
    networks:
      - elastic

  melkradar-adver-cluster-live-adver-2:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.0
    container_name: melkradar-adver-cluster-live-adver-2
    environment:
      - node.name=melkradar-adver-cluster-live-adver-2
      - cluster.name=melkradar-adver-cluster-live-adver
      - discovery.seed_hosts=melkradar-adver-cluster-live-adver-0,melkradar-adver-cluster-live-adver-1
      - cluster.initial_master_nodes=melkradar-adver-cluster-live-adver-0,melkradar-adver-cluster-live-adver-1,melkradar-adver-cluster-live-adver-2
      - bootstrap.memory_lock=true
      - xpack.security.enabled=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "xpack.security.transport.ssl.enabled=true"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic

  kibana-adver-kibana:
    image: docker.elastic.co/kibana/kibana:7.17.0
    container_name: kibana-adver-kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://melkradar-adver-cluster-live-adver-0:9200
    ports:
      - 5601:5601
    networks:
      - elastic

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge
```

#### Step 2: Run the Docker Compose
Open a terminal in the same directory as your `docker-compose.yml` file and run the following command:

```bash
docker-compose up -d
```

This command will start the Elasticsearch cluster and Kibana in the background. The `-d` flag detaches the process.

#### Step 3: Check Cluster Status
To check the status of the Elasticsearch cluster, run the following command:

```bash
curl -X GET "http://localhost:9200/_cluster/health?pretty"
```

This command will display health indicators of the Elasticsearch cluster.

#### Step 4: Manage Files
Now you can attach your necessary files to the cluster. You can use tools like Logstash or create files directly in Elasticsearch.

#### Step 5: Stop and Clean Up
When you're done, stop the processes and release resources:

```bash
docker-compose down
```

This command will stop all services and remove containers, networks, and volumes.

Now you have a basic Elasticsearch cluster up and running using Docker Compose. Customize the configurations as needed for your development or production environment. For additional security or configuration options, refer to the Elasticsearch documentation.
