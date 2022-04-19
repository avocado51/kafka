# TEST Environment Kafka Cluster in Docker
> Docker version up to 20.10.6 \
Docker-compose version up to version 1.24.0

- exec docker-compose 

```
$ docker-compose up -d --build

// make cluster ( server count = 3 )
$ docker-compose scale kafka=3

$ docker ps

// print kafka log
$ docker container logs [kafka_container]
$ docker container logs [zookeeper_container]

// connect container
$ docker exec -it [kafka_container] bin/bash
```

- test kafka producer and consumer

```
$ kafka-topics.sh --list --bootstrap-server 192.168.10.160:9092

// create topic
$ bin/kafka-topics.sh --create --bootstrap-server 192.168.10.160:49162,192.168.10.160:49163 --replication-factor 2 --partitions 5 --topic test_topic

// start producer request message
$ kafka-console-producer.sh --broker-list 192.168.10.160:49162 --topic test_topic

// start consumer (* test in another console *)
$ kafka-console-consumer.sh --bootstrap-server 192.168.10.160:49162 --topic test_topic --from-beginning
```
