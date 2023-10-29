#  How to monitor your kafka connectors 

This a quick set up of  kafka, kafka ui and kafka connect with a monitoring through Prometheus and Grafana using docker compose.

# Architecture


# Setup

**Required software:**

* Docker, Docker Compose.
* Portainer (not required). 

Here we are using a mongo kafka sink connector. Jar is downloaded from [here](https://repo1.maven.org/maven2/org/mongodb/kafka/mongo-kafka-connect/1.11.0/) and put in the kafka-jars.
We are using jmx exporter to collect kafka connect metrics. ( see the prometheus directory)
Metrics config and grafana dashboard are download from [here](https://github.com/confluentinc/kafka-connect-monitoring-sandbox) and put in grafana directory. 

**docker compose :**

To build and deploy, we run this command:

```shell script
docker-compose up -d
```
kafka & zookeeper, akhq, mongo database, kafkacat, schema-registry, prometheus, grafana containers will be created.
![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/4a5b8b64-8aeb-45cb-a570-211a10900cf6)

And we can see with akhq that our topics are created:

![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/2a1d1410-dedf-44b2-9ec7-a19be602d410)

Prometheus is up and if we go to the status http://localhost:9090/targets?search= 

![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/323c715f-3c71-4f5c-a6a9-ae3da8fce315)

**Create connector with Rest API:**

Inside the container, we use the Post method to create 2 connectors

```shell script
curl -X POST -H "Content-Type: application/json" -d '{ "name":"connector-json-1", "config":{"connector.class":"com.mongodb.kafka.connect.MongoSinkConnector","tasks.max":"1","topics":"json","collection":"json1","internal.key.converter.schemas.enable":"false","key.converter.schemas.enable":"false","database":"test","connection.uri":"mongodb://admin:admin@host.docker.internal:27017","value.converter.schemas.enable":"false","value.converter":"org.apache.kafka.connect.storage.StringConverter"}}' "localhost:8083/connectors"
```

```shell script
curl -X POST -H "Content-Type: application/json" -d '{ "name":"connector-json-2", "config":{"connector.class":"com.mongodb.kafka.connect.MongoSinkConnector","tasks.max":"1","topics":"json","collection":"json2","internal.key.converter.schemas.enable":"false","key.converter.schemas.enable":"false","database":"test","connection.uri":"mongodb://admin:admin@host.docker.internal:27017","value.converter.schemas.enable":"false","value.converter":"org.apache.kafka.connect.storage.StringConverter"}}' "localhost:8083/connectors"
```

**Create prometheus datasource and import dashboard template :**

Finally, to monitor the kafka connector: go to Grafana http://localhost:3000:

* First create a prometheus datasource 
![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/9e1b0fe1-a015-41a4-8418-9de54a82e89b)

* Next, create the monitoring dashboard by importing the template located in grafana directory.

![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/a8476d18-2499-4dba-bc54-564b3b3d1794)

![image](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/cf28383e-8846-4eeb-aaa4-a694e33cd6e7)

