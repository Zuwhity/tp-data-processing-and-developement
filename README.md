# Practices - Data engineering

Have a stackoverflow account : https://stackoverflow.com/

Have a github account : https://github.com/

And a github repo to push your code.

## Fork the repo on your own Github account
* https://github.com/polomarcus/tp/fork

## Docker and Compose 
Take time to read and install

https://docs.docker.com/get-started/overview/
```
docker --version
Docker version 20.10.14
```

https://docs.docker.com/compose/
```
docker-compose --version
docker-compose version 1.29.2
```

## [Apache Kafka](https://kafka.apache.org/)
### Communication problems
![](https://content.linkedin.com/content/dam/engineering/en-us/blog/migrated/datapipeline_complex.png)


### Why Kafka ?
https://kafka.apache.org/documentation/#introduction

Answer these questions with what you can find on the documentation :

- What problems does Kafka solve ?
   Here kafka help to solve some communication problem by centralising all the request in one put.
   
- Which use cases ?
    More ever kafka is to : - To publish (write) and subscribe to (read) streams of events, including continuous import/export of your data from other systems.
                            - To store streams of events durably and reliably for as long as you want.
                            - To process streams of events as they occur or retrospectively.

- What is a producer and consumer ?
    Producers are those client applications that publish (write) events to Kafka, and consumers are those that subscribe to (read and process) these events. In Kafka, producers and consumers are fully decoupled and agnostic of each other, which is a key design element to achieve the high scalability that Kafka is known for. 

- What are consumer groups ?
    send a message to a certain group of people by their ID

- What is a offset ?

- Why using partitions ? 
      we have a sort of conversation / discution with all the message around the topics.

- Why using replication ?

- What are  In-Sync Replicas (ISR) ?


![](https://content.linkedin.com/content/dam/engineering/en-us/blog/migrated/datapipeline_simple.png)

### Try to install Kafka without docker
https://kafka.apache.org/documentation/#gettingStarted

### Use kafka with docker
Start multiples kakfa servers (called brokers) by downloading a docker compose recipe : 
* https://github.com/conduktor/kafka-stack-docker-compose#single-zookeeper--multiple-kafka

Check on the docker hub the image used : 
* https://hub.docker.com/r/confluentinc/cp-kafka


### Verify
```
docker ps
CONTAINER ID   IMAGE                             COMMAND                  CREATED          STATUS         PORTS                                                                                  NAMES
b015e1d06372   confluentinc/cp-kafka:7.0.1       "/etc/confluent/dock…"   10 seconds ago   Up 9 seconds   0.0.0.0:9092->9092/tcp, :::9092->9092/tcp, 0.0.0.0:9999->9999/tcp, :::9999->9999/tcp   kafka1
(...)
```

### Getting started with Kafka
1. Connect to your kafka cluster with 2 command-line-interface (CLI)

Using [Docker exec](https://docs.docker.com/engine/reference/commandline/exec/#description)

```
docker exec -ti my_kafka_container_name bash
> pwd
```

```
> kafka-topics 
# will give you help to use this command
> kafka-topics --describe --bootstrap-server localhost:9092 
# will give you an error
```
Read this blog article to fix `Broker may not be available.` error : https://rmoff.net/2018/08/02/kafka-listeners-explained/

Pay attention to the `KAFKA_ADVERTISED_LISTENERS` config from the docker-compose file.

2. Create a "mailbox" - a topic with the default config : https://kafka.apache.org/documentation/#quickstart_createtopic
3. Check on which Kafka broker the topic is located using `--describe`
5. Send events to a topic on one terminal : https://kafka.apache.org/documentation/#quickstart_send
4. Keep reading events from a topic from one terminal : https://kafka.apache.org/documentation/#quickstart_consume
* try the default config
* what does the `--from-beginning` config do ?
*     If the consumer does not already have  an established offset to consume from, start with the earliest message present in the log rather than the latest message.
* what about using the `--group` option for your producer ?
* The consumer group id of the consumer
6. stop reading
7. Keep sending some messages to the topic

#### Partition 
1. Check consumer group with `kafka-console-consumer` : https://kafka.apache.org/documentation/#basic_ops_consumer_group
* notice if there is [lag](https://univalence.io/blog/articles/kafka-et-les-groupes-de-consommateurs/) for your group
2. read from a new group, what happened ? there is not the old message print in the console , it's like a page mark for books
3. read from a already existing group, what happened ? here we have some message in lag so it will show the message that are hide when we run this groups again 
4. Recheck consumer group , we have now multiple group in our list and so it's a nice way share and split the code

#### Replication - High Availability
1. Increase replication in case one of your broker goes down : https://kafka.apache.org/documentation/#topicconfigs
2. Stop one of your brokers with docker
3. Describe your topic, check the ISR (in-sync replica) config : https://kafka.apache.org/documentation/#design_ha
4. Restart your stopped broker
5. Check again your topic

### Using a Scala application with Kafka Streams to read and write to Kafka
* Scala https://docs.scala-lang.org/getting-started/index.html
* Scala build tool : https://www.scala-sbt.org/download.html
* https://kafka.apache.org/documentation/streams/
* https://github.com/polomarcus/Spark-Structured-Streaming-Examples

### Continuous Integration (CI)
If it works on your machine, congrats. Test it on a remote servers now thanks to a Continuous Integration (CI) system such as [GitHub Actions](https://github.com/features/actions) : 
* How to use containers inside a CI : https://docs.github.com/en/github-ae@latest/actions/using-containerized-services/about-service-containers
* A Github Action example : https://github.com/conduktor/kafka-stack-docker-compose/blob/master/.github/workflows/main.yml

# Tools
* Scala IDE : https://www.jetbrains.com/idea/
* Kafka User Interface (UI) : https://www.conduktor.io/download/
