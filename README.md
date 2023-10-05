</br>
<h1 align="center">Structured Streaming of Sensor Data</h1>
</br>
<p align="center">This project consist in the creation of a pipeline for the flow of sensor data from an Arduino console to a Databricks Delta Table, where a Spark cluster can be used for real-time data processing and analysis.</p>

</br>

<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/Structured%20Streaming%20Ironhack%20Project.png">
</br>


## Arduino
Arduino is an open-source electronics platform that consists of both hardware and software components, designed for creating interactive and programmable electronic projects. Sensors used:
- DHT11: for temperature and humidity observations
- SR04: sonar sensor for high frequence measuring of the nearest object distance
- PIR: Passive Infra-Red sensor for movement detection
</br>
<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/arduino.jpeg">
</br>


## EMQX & MQTTX
MQTT is a lightweight, publish-subscribe, machine to machine network protocol for message queuing service. It is designed for connections with remote locations that have devices with resource constraints or limited network bandwidth, such as in the Internet of Things.
EMQX is an open-source, distributed MQTT messaging broker that can support millions of concurrent MQTT connections.

In this project an EMQX Cloud service enabled the connection of the Arduino signals to the Kafka Server thanks to the Data Bridge to Kafka feature.
The connection could then be easily tested with the help of the MQTTX client toolbox for desktop.

</br>
<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/emqx_data_bridge.png">
</br>


## Kafka
Kafka is an open-source distributed event streaming platform that allows real-time data ingestion, processing, and distribution across applications and systems.

Kafka was installed on a AWS EC2 instance through an SSH connection and its VPC was then Peer Connected to the Databricks and EMQX ones, allowing comunication between the systems.
Four different topics where created for each type of measurement so that the messages could asynchronously flow to the destination.

```sh
# Start the ZooKeeper service
$ bin/zookeeper-server-start.sh config/zookeeper.properties

# Start the Kafka broker service
$ bin/kafka-server-start.sh config/server.properties
```
</br>

## AWS
Cloud computing platform that provides a wide range of scalable and flexible cloud services and infrastructure.

For this project AWS served as the Cloud Service Provider for the Databricks environment, for the Kafka broker and for the EMQX Cloud service. The three resulting VPC where then Peer Connected so that, after configuring the necessary Route Tables and Security Groups, the respective compute instances could seamlessy comunicate with one another using their private IPs.

</br>
<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/aws_inbound_rules.png">
</br>

## Spark
Apache Spark is an open-source unified analytics engine for large-scale data processing. It provides an interface for programming clusters with implicit data parallelism and fault tolerance.

The Spark cluster allowed the start of read and write streams that respectively tuned to the message queues of the different Kafka topics and moved the payload values to a Catalog Delta Table, in which the data was finally stored in a Delta Lake format.

```python
# Start a readStream from the Kafka Server
pir_stream = (spark.readStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "10.127.12.3:9092")
  .option("subscribe", "pir_test")
  .option("startingOffsets", "latest")
  .load()
)

# Start a writeStream to a Delta Table
(pir_stream
    .selectExpr(
        "DATE_FORMAT(timestamp, 'dd/MM/yyyy HH:mm:ss') as timestamp",
        "'new detection' as movement"
    )
    .writeStream
    .format("delta")
    .outputMode("append")
    .option("checkpointLocation", "/tmp/delta/kafka_test/pir_checkpoint/")
    .toTable("data_pipelines.kafka_table.pir_stream_test")
)
```
</br>


## Databricks
Databricks is a comprehensive, cloud-based (AWS) data analytics and machine learning platform that offers the scalability and automation needed to handle complex data workflows.

The Databricks Notebook's versatility and ease of use were leveraged to test the Kafka connection and start the readStream and writeStream tasks. Databriks offered a platform in which it was possible not only to integrate different systems and technologies, but also to perform basic analysis to uncover some of the data's trends and insights.

</br>
<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/dashboard_temp_hum.png">
</br>
</br>
