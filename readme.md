# Kafka Producer and Consumer Example with Headers 
This example demonstrates a simple Kafka producer and consumer in Java that processes messages in batches and includes headers.

## Prerequisites
-Java 8 or higher
-Apache Maven
-Apache Kafka
-Installing Kafka
-Follow these steps to install Kafka on your local machine:

### For macOS
Install Homebrew if not already installed: https://brew.sh/

Install Kafka using Homebrew:

```sh
brew install kafka
```

### For Linux (Debian/Ubuntu)
Download Kafka from the official website: https://kafka.apache.org/downloads

Extract the downloaded archive:

```sh
tar -xzf kafka_2.13-2.8.1.tgz
cd kafka_2.13-2.8.1
```

### For Windows
Install Windows Subsystem for Linux (WSL) by following the instructions here: https://docs.microsoft.com/en-us/windows/wsl/install
Install a Linux distribution, such as Ubuntu, from the Microsoft Store
Launch your Linux distribution and follow the Linux installation steps mentioned above

## Running the Example
### 1. Start Zookeeper and Kafka Broker


For macOS  

In separate terminal windows, run the following commands:

```sh
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
kafka-server-start /usr/local/etc/kafka/server.properties
```

For Linux (Debian/Ubuntu)

In separate terminal windows, run the following commands:

```sh
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
./bin/kafka-server-start.sh ./config/server.properties
```

### 2. Create the Kafka Topic

Run the following command to create a topic named "example-topic":

```sh
kafka-topics --create --topic example-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

### 3. Setup OpenTelemetry variables

Set the JAVA_TOOL_OPTIONS to use the OpenTelemetry Java agent and export to Honeycomb

```sh
export JAVA_TOOL_OPTIONS="-javaagent:./opentelemetry-javaagent.jar"
export OTEL_TRACES_EXPORTER="otlp"
export OTEL_METRICS_EXPORTER="none"
export OTEL_EXPORTER_OTLP_ENDPOINT="https://api.honeycomb.io"
export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=$HONEYCOMB_API_KEY"
```

### 4. Compile and Run the Producer and Consumer

Open a terminal and navigate to the kafka-example folder. Run the following Maven commands:

```sh
# Compile the project
mvn clean compile

# Run the Kafka producer
mvn exec:java -Dexec.mainClass="com.example.kafka.BatchProducerWithHeader"

# In a separate terminal, run the Kafka consumer
mvn exec:java -Dexec.mainClass="com.example.kafka.BatchConsumerWithHeader"
```

The producer will send 100 messages with headers to the "example-topic" topic. The consumer will subscribe to the same topic and read messages in batches of 10, printing the message details along with the headers.

Example trace with header set:
![Alt text](/example.png?raw=true "Example Trace")