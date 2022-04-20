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
$ kafka-topics.sh --list --bootstrap-server {public ip}:{PORT_COMMAND}

// create topic
$ bin/kafka-topics.sh --create --bootstrap-server {public ip}:{PORT_COMMAND} --replication-factor 2 --partitions 5 --topic {TOPIC NAME}

// start producer request message
$ kafka-console-producer.sh --broker-list {public ip}:{PORT_COMMAND} --topic {TOPIC NAME}

// start consumer (* test in another console *)
$ kafka-console-consumer.sh --bootstrap-server {public ip}:{PORT_COMMAND} --topic {TOPIC NAME} --from-beginning
```
