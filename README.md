# Structured Streaming of Sensor Data
This project consist in the creation of a pipeline for the flow of sensor data from an Arduino console to a cloud-based storage location (Databricks Delta Table), where a Spark cluster can be used for real-time data processing and analysis.

<img width="auto" alt="image" src="https://github.com/Frussen/structured_streaming_sensor_data/blob/main/files/Structured%20Streaming%20Ironhack%20Project.png">


### Arduino
Arduino is an open-source electronics platform that consists of both hardware and software components, designed for creating interactive and programmable electronic projects. Sensors used:
- DHT11: for temperature and humidity observations
- SR04: sonar sensor for high frequence measuring of the nearest object distance
- PIR: Passive Infra-Red sensor for movement detection

</br>

### Kafka
Kafka is an open-source distributed event streaming platform that allows real-time data ingestion, processing, and distribution across applications and systems.


### Spark
Apache Spark is an open-source unified analytics engine for large-scale data processing. It provides an interface for programming clusters with implicit data parallelism and fault tolerance.


### AWS
Cloud computing platform that provides a wide range of scalable and flexible cloud services and infrastructure.


### Databricks
Databricks is a comprehensive, cloud-based (AWS) data analytics and machine learning platform that offers the scalability and automation needed to handle complex data workflows.
Databricks provides a collaborative workspace where data engineers, data scientists, and analysts can work together to explore data, build data pipelines, develop machine learning models, and gain insights from their data.

