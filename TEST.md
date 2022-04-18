# TEST Environment Kafka Cluster in Docker
> Docker version up to 20.10.6 \
Docker-compose version up to version 1.24.0

- docker-compose.yml

```
version: '2'
services:
  zookeeper:
    container_name: local-zookeeper
    image: wurstmeister/zookeeper:3.4.6
    ports:
      - "2181:2181"
  kafka:
    container_name: local-kafka
    image: wurstmeister/kafka:2.12-2.3.0
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1
      KAFKA_ADVERTISED_PORT: 9092
      #KAFKA_CREATE_TOPICS: "test_topic:2:1" # Topic명:Partition개수:Replica개수
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

```

- exec docker-compose 

```
$ docker-compose up -d
// print kafka log 
$ docker container logs local-kafka
$ docker container logs local-zookeeper

// connect container
$ docker exec -it local-kafka bin/bash
```

- test kafka producer and consumer

```
$ cd /opt
$ kafka-topics.sh --list --bootstrap-server localhost:9092
// create topic
$ bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 5 --topic test_topic 

// start producer message
$ kafka-console-producer.sh --broker-list localhost:9092 --topic test_topic

// start consumer (* another console *)
$ kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test_topic --from-beginning
```
