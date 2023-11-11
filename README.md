#  How to monitor your kafka connectors 

This a quick set up of  kafka, kafka ui and kafka connect with a monitoring through Prometheus and Grafana using docker compose.

# Architecture
![architetcure](https://github.com/SaraMaz/kafka-connect-monitoring/assets/20047882/73b16453-804f-4963-98c4-08b797ec1a19)

# Setup

**Required software:**

* Docker, Docker Compose.

Here we are using a mongo kafka sink connector. Jar is downloaded from [here](https://repo1.maven.org/maven2/org/mongodb/kafka/mongo-kafka-connect/1.11.0/) and put in the kafka-jars.

We are using jmx exporter to collect kafka connect metrics.

Metrics config and grafana dashboard are download from [here](https://github.com/confluentinc/kafka-connect-monitoring-sandbox). 

# References

See this [article](https://medium.com/@mengineer/kafka-connect-how-to-add-monitoring-with-prometheus-and-grafana-2eeb516e88a3) for more explanation.

