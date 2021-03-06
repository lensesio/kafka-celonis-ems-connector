# Celonis EMS Kafka connector

<img src="./images/apps_kafka_ems.png" width="100%"/>

## Introduction
A Kafka Connect sink connector allowing data stored in Apache Kafka to be uploaded to Celonis Execution Management System (EMS) for process mining and execution automation.

## Full Documentation

See the [Wiki]([https://github.com/lensesio/kafka-ems-connector/wiki](https://github.com/celonis/kafka-ems-connector/pull/40)) for full documentation, examples, operational details and other information.

## Requirements

Apache Kafka at least 2.3 version and a Kafka Connect cluster.


## Installation

### Manual

To download the connector please register [here](https://lenses.io/connect/kafka-to-celonis/). If you are already a Lenses customer, this connector will be made available to your client area.

* Download the connector ZIP file. If you're running Kafka version 2.5 or lower, use the 2.12 archive otherwise use the 2.13 one.
* Extract the ZIP file contents and copy the contents to the desired location. For example, you can create a directory named <path-to-kafka-installation>/share/kafka/plugins then copy the connector plugin contents.
* Add this to the plugin path in your Connect worker properties file. Kafka Connect finds the plugins using its plugin path. A plugin path is a comma-separated list of directories defined in the Kafka Connect worker's configuration. This might already be set up and therefore there is nothing to do.
  For example:
```
plugin.path=/usr/local/share/kafka/plugins
``` 

* Start the Kafka Connect workers with that configuration. Connect will discover all connectors defined within those plugins.
* Repeat these steps for each machine where Connect is running. Each connector must be available on each worker.


### MSK Connect

To create the connector follow the steps provided [here](https://docs.aws.amazon.com/msk/latest/developerguide/msk-connect-connectors.html). 
The steps involved will require installing a custom connector and for that follow this [link](https://docs.aws.amazon.com/msk/latest/developerguide/msk-connect-plugins.html) and use the connector release jar

### Confluent 

Our connector is available on [Confluent Hub](https://docs.confluent.io/home/connect/self-managed/install.html#). Follow the instructions [here](https://docs.confluent.io/home/connect/self-managed/install.html#) to enable the EMS Kafka Connect sink.

### Azure Event Hub 

If you're running Event Hub, you can leverage Kafka Connect and the EMS Sink plugin to load data into the EMS platform. 
The instructions to enable Kafka Connect for Event Hub can be found [here](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-kafka-connect-tutorial).
Once the installation is done, follow the manual steps to enable the connector before creating an instance of it.


## Sample connector configuration

Here is a sample configuration for the connector:

```
name=kafka2ems
connector.class=com.celonis.kafka.connect.ems.sink.EmsSinkConnector
tasks.max=1
key.converter=org.apache.kafka.connect.storage.StringConverter
value.converter=org.apache.kafka.connect.json.JsonConverter
topics=payments 
connect.ems.endpoint=https://***.***.celonis.cloud/continuous-batch-processing/api/v1/***/items
connect.ems.target.table=payments
connect.ems.connection.id=****
connect.ems.commit.size.bytes=10000000
connect.ems.commit.records=100000
connect.ems.commit.interval.ms=30000
connect.ems.tmp.dir=/tmp/ems
connect.ems.authorization.key="AppKey ***"
connect.ems.error.policy=RETRY
connect.ems.max.retries=20
connect.ems.retry.interval=60000
connect.ems.parquet.write.flush.records=1000
connect.ems.debug.keep.parquet.files=false
```
The full list of configuration keys can be found in the wiki.


## Bugs and Feedback
For bugs, questions and discussions please use the GitHub Issues.

## License
This code is open source software licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0.html).
