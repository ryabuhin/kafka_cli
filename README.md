# Kafka CLI

Kafka CLI is a self-documented command-line interface designed to simplify the integration and operation of kafka and other kafka-related services.

## Features

* Main features from the root menu of CLI: 

```
Usage: kfk <command> [<subcommand>] [<parameters>]
These are the available commands:
    consume     Consume data from topics.
    produce     Produce data to topics.
    brokers     Manage kafka brokers.
    cgroups     Manage consumer groups.
    sregistry   Manage schema registry service.
    topics      Manage kafka topics.
    system      Analze kafka cluster and maintain the environment replication.
    logging     Administer the logging of kafka services.
    tunnel      Manage SSH tunnels from local machine to remote kafka clusters.
    version     Print the kfk-cli version or the individual version of a local services.

See 'kfk help <command>' to read about a specific command.
```
* Work with kafka using kafka binaries:
  * consume, produce messages and replicate data
  * manage kafka brokers
  * operate with consumer groups
  * create, delete and replicate topics
* Manage schema registry services.
* The ability to configure logger.
* Simple control of the local-environments:

<p align="center"><img src="/img/sle.gif?raw=true"/></p>

* Managing of the SSH tunnels to work locally with remote environments:

<p align="center"><img src="/img/tdev.gif?raw=true"/></p>

## Getting started

Before starting you need:

> In this case, my approach with versioning of development tools is demonstrated. You can skip versioning and install Apache Kafka on your own, 
just do not forget KAFKA_HOME and PATH variables.

* Download and install Apache Kafka
* Create symbolik links for the `current` version of apache kafka
* Set `KAFKA_HOME` environment variable that contains path to `current` version of apache kafka
* Add `${KAFKA_HOME}/bin` directory into the `${PATH}`
* Install Kafka CLI `(kfk)`

To complete all of these steps, do the following:
```
kafka_version=3.1.0
curl "https://dlcdn.apache.org/kafka/${kafka_version}/kafka_2.13-${kafka_version}.tgz" -o "kafka_${kafka_version}.tgz"
tar zxvf kafka_${kafka_version}.tgz
sudo mkdir -p /usr/local/opt/kafka/
sudo mv "./kafka_2.13-${kafka_version}" /usr/local/opt/kafka/${kafka_version}
# make sure that current user is owner of the kafka directory
sudo chown -R $(whoami) /usr/local/opt/kafka
# create symbolic link for ${kafka_version} -> current
ln -s /usr/local/opt/kafka/${kafka_version} /usr/local/opt/kafka/current
```

Create `KAFKA_HOME` environment variable and add kafka binaries `${KAFKA_HOME}/bin` into the `${PATH}`. 
Modify `${HOME}/.bashrc` file as follows:

> Or modify `${HOME}/.profile` if you are macOS user.

```
echo 'export KAFKA_VER_DIR="/usr/local/opt/kafka"' >> ${HOME}/.bashrc
echo 'export KAFKA_HOME="${KAFKA_VER_DIR}/current"' >> "${HOME}/.bashrc"
echo 'export PATH="$PATH:$KAFKA_HOME/bin"' >> "${HOME}/.bashrc"
echo "switchto-kafka310=\"rm -rf $KAFKA_HOME && ln -s ${KAFKA_VER_DIR}/3.1.0 $KAFKA_HOME && echo '> Kafka v3.1.0 is active.'
``` 

Download `kfk` CLI using git, store it `${HOME}/.kafka_cli` directory and create `KFK_CLI_HOME` environment variable.

```
git clone https://github.com/ryabuhin/kafka_cli.git ${HOME}/.kafka_cli
echo 'export KFK_CLI_HOME="${HOME}/.kafka_cli"' >> ${HOME}/.bashrc
```

## Dependencies

Required software and libraries for working with Kafka CLI `(kfk)`:

* apache kafka >= 3.0.0
* python >= 3.7.6
* bash >= v3.2
* jq >= v1.5.1
* visidata >= 2.4
* docker >= 20.10
* docker-compose >= 1.29.2
* curl
* git

## Configure environments

Before running the kfk CLI on specific environment you need to be sure that you have prepared property file in the `${KFK_CLI_HOME}/etc/env` directory. The pattern of environment property file looks like:

```
#
## connection settings
ssl.endpoint.identification.algorithm=https
sasl.mechanism=PLAIN
request.timeout.ms=20000
bootstrap.servers=<BOOTSTRAP SERVERS>
retry.backoff.ms=500
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<SASL JAAS USERNAME>" password="SASL JAAS PASSWORD";
security.protocol=SASL_SSL

# schema registry configs
basic.auth.credentials.source=USER_INFO
schema.registry.basic.auth.user.info=<SCHEMA REGISTRY BASIC AUTH CREDS>
schema.registry.url=<SCHEMA REGISTRY URL>

# string and apache avro serializers (optional)
key.serializer=org.apache.kafka.common.serialization.StringSerializer
value.serializer=io.confluent.kafka.serializers.KafkaAvroSerializer
```

Find more examples of actual properties files for local-development purposes here: **${KFK_CLI_HOME}/etc/env/local-.*_config.properties**.