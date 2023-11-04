

# JDBC Sink Demo | MySQL 

Supporting document for youtube video 

## Initial Set Up


* [Install MySQL](https://formulae.brew.sh/formula/mysql)
* [MySQL workbench](https://downloads.mysql.com/archives/workbench/)
* [Install Kafka](https://kafka.apache.org/quickstart)
* [Install Kafka JDBC sink Connector](https://www.confluent.io/hub/confluentinc/kafka-connect-jdbc)
* [Install Kafka Json Schema Convertor](https://www.confluent.io/hub/confluentinc/kafka-connect-json-schema-converter)
* [Install Docker](https://docs.docker.com/desktop/install/mac-install/)
* [Install schema registery](https://hub.docker.com/r/confluentinc/cp-schema-registry) Or [Without Docker](https://github.com/confluentinc/schema-registry)

## Configuring JDBC Sink


1. Start Kafka
```shell
bin/zookeeper-server-start.sh config/zookeeper.properties 
bin/kafka-server-start.sh config/server.properties
bin/kafka-topics.sh --create --topic jdbc-sink-topic --bootstrap-server localhost:9092
```
2. Start Mysql
```shell
brew services start mysql
```
3. Start Schema Registry
```shell
docker pull confluentinc/cp-schema-registry
```

```shell
docker run -p 8081:8081 -d \
  --name=schema-registry \
  -e SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS=host.docker.internal:9092 \
  -e SCHEMA_REGISTRY_HOST_NAME=localhost \
  -e SCHEMA_REGISTRY_LISTENERS=http://0.0.0.0:8081  \
  -e SCHEMA_REGISTRY_DEBUG=true \
  confluentinc/cp-schema-registry:latest
```

4. Update Default sink properties with MySql Configuration
```shell

connection.url=jdbc:mysql://localhost:3306/demo?user=root

value.converter.schemas.enable=true
value.converter=io.confluent.connect.json.JsonSchemaConverter
value.converter.schema.registry.url=http://localhost:8081
key.converter=org.apache.kafka.connect.storage.StringConverter
key.converter.schemas.enable=true
```

5. Place mysql connector jar in confluentinc-kafka-connect-jdbc-10.7.4/lib folder

6. Start JDBC sink in Standalone mode.
```shell
bin/connect-standalone.sh config/connect-standalone.properties /{PATH}/confluentinc-kafka-connect-jdbc-10.7.4/etc/sink-quickstart-sqlite.properties 
```

7. Push Json Message in Kafka topic by Kafka console producer providing by Confluent. Schema will automatically created and 
default strategy used in TopicRecordNameStrategy

```shell

kafka-json-schema-console-producer --bootstrap-server host.docker.internal:9092  \
--property schema.registry.url=http://localhost:8081 --topic jdbc-sink-topic \
--property value.schema='{"type":"object", "properties":{"id":{"type":"string"},"name":{"type":"string"} }}'
```

```shell
{ "id":1, "name":"kritika" }
```
```shell
curl --location --request GET 'http://localhost:8081/schemas/'
```
8. Check My Sql db , you will have an entry
