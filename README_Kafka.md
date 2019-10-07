# 1. what is Apache Kafka
Apache kafka is a distributed streaming platform specialized for large-capacity, real-time log processing. Kafka can process data with a stable architecture and fast performance in a messaging system whose primary purpose is to deliver data securely and without loss. Apache Zookeeper is responsible for distributed processing of brokers within the Apache Kafka cluster. Apache Kafka's broker manages messages based on topic. Producer generates a message from a specific topic and then forwards the message to the broker. When the broker groups and stacks up the messages it receives, it is taken and processed by consumers who subscribe to the topic.

![explan spark concept](img/kafka_Architecture.jpg)

# 2. Setup
We strongly recommend forming at least three Kafka breakers.

## 2.1 Installing Kafka on Linux

1\. Go to [Apache Kafka Download page](https://kafka.apache.org/downloads). Choose the Spark release (2.3.0). Click on the link "Download Kafka" to get the `tgz` package of the 2.3.0 Spark release. We will be using that in the rest of these guidelines but feel free to adapt to your version.

```
wget http://www-eu.apache.org/dist/kafka/2.3.0/kafka_2.11-2.3.0.tgz

```

2\. Uncompress that file into `/usr/local` by typing:

```
sudo tar xvzf kafka_2.11-2.3.0.tgz -C /usr/local/
```

3\. Create a shorter symlink of the directory that was just created using:

```
sudo ln -s /usr/local/kafka_2.11-2.3.0 /usr/local/kafka
```
## 2.2 Installing Kafka on Windows 
 1. Click on the link, below to download. 
 ```
 http://www-eu.apache.org/dist/kafka/2.3.0/kafka_2.11-2.3.0.tgz
 ```
2. Unzip it.

## 2.3 kafka Settings.
The settings are the same for both Windows and Linux.<br>

### 2.3.1 kafka with Zookeeper 
Edit Zookeeper properties into `/usr/local/kafka_2.11-2.3.0/config/` by typing:

```bash
# /usr/local/kafka_2.11-2.3.0/config/zookeeper.properties

dataDir=/tmp/zookeeper
clientPort=2181
maxClientCnxns=0
initLimit=10
syncLimit=5

broker1=broker1_IP:2888:3888
broker2=broker2_IP:2888:3888
broker3=broker3_IP:2888:3888
```
Edit Kafka properties into `/usr/local/kafka_2.11-2.3.0/config/` by typing:

```bash
# /usr/local/kafka_2.11-2.3.0/config/server.properties

broker.id=broker1

listeners=PLAINTEXT://:9092
advertised.listeners=PLAINTEXT://broker1_IP:9092
advertised.host.name=broker1_IP
advertised.host.name=9092

zookeeper.connect=broker1_IP:2181

zookeeper.connection.timeout.ms=6000
```

### 2.3.2 kafka for broker

Edit Kafka properties into `/usr/local/kafka_2.11-2.3.0/config/` by typing:

```bash
# /usr/local/kafka_2.11-2.3.0/config/server.properties

broker.id=broker2

listeners=PLAINTEXT://:9092
advertised.listeners=PLAINTEXT://broker2_IP:9092
advertised.host.name=broker2_IP
advertised.host.name=9092

zookeeper.connect=broker1_IP:2181

zookeeper.connection.timeout.ms=6000
```


## 2.4 Run

### 2.4.1 For Linux
To run Zookeeper

```bash
# /usr/local/kafka_2.11-2.3.0/
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

To run Kafka

```bash
# /usr/local/kafka_2.11-2.3.0/
$ bin/kafka-server-start.sh config/server.properties
```

To generate topic

```bash
# /usr/local/kafka_2.11-2.3.0/
/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic bsnc
```

To check generated topic
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-topics.sh --list --zookeeper localhost:2181
```

### 2.4.2 For Windows
To run Zookeeper

```bash
# /usr/local/kafka_2.11-2.3.0/
$ bin/zookeeper-server-start.bat config/zookeeper.properties
```

To run Kafka

```bash
# /usr/local/kafka_2.11-2.3.0/
$ bin/kafka-server-start.bat config/server.properties
```

To generate topic

```bash
# /usr/local/kafka_2.11-2.3.0/
/bin/kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic bsnc
```

To check generated topic
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-topics.bat --list --zookeeper localhost:2181
```

## 2.5 Test

### 2.5.1 For Linux

Run Producer
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic bsnc
```

Run Consumer
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bsnc --from-beginning
```

### 2.5.1 For Linux

Run Producer
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-console-producer.bat --broker-list localhost:9092 --topic bsnc
```

Run Consumer
```bash
# /usr/local/kafka_2.11-2.3.0/
bin/kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic bsnc --from-beginning
```