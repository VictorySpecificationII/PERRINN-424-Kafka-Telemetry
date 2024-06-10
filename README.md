\# Solution Design

# Introduction

The solution architecture outlined in this document aims to establish a robust and scalable framework for building a high-throughput Big Data pipeline dedicated to telemetry acquisition and processing. Designed to support the needs of performance engineers and machine learning engineers, this architecture facilitates real-time monitoring and analysis of vehicle data to drive continuous improvement and innovation in automotive technologies.

## Purpose and Goals

The primary purpose of this architecture is to create a development environment tailored specifically for telemetry data from the PERRINN 424 Race Car. Key goals of the architecture include:

 - High Throughput Data Processing: Implementing efficient data ingestion, processing, and storage mechanisms to handle the large volume of telemetry data generated by vehicles.
 - Real-time Monitoring and Analysis: Enabling real-time monitoring of vehicle performance metrics and facilitating data-driven insights for performance optimization.
 - Scalability and Flexibility: Building a scalable and flexible architecture capable of accommodating diverse data sources, processing workflows, and analytical tasks.
 - Integration of Machine Learning: Providing support for integrating machine learning models and algorithms to analyze telemetry data, identify patterns, and make predictive insights for performance enhancement.
 - User-Friendly Interfaces: Incorporating user-friendly interfaces and visualization tools to facilitate easy exploration, visualization, and interpretation of telemetry data by engineers and analysts.

By achieving these goals, the architecture empowers the community to collaboratively monitor, analyze, and improve vehicle performance and functionality through data-driven insights and innovations.

# Infrastructure Overview

The infrastructure for the solution is built upon Docker containers, providing a lightweight and portable environment for developing and testing various components of the telemetry acquisition and processing pipeline. The architecture consists of several key components, each serving a specific purpose in the data processing workflow.

### Component Overview

1. **Zookeeper**:
   - **Description**: Responsible for coordination and synchronization between distributed systems, such as Kafka and HBase.
   - **Usage**: Ensures reliable communication and management of distributed resources across the ecosystem.

2. **Zoonavigator**:
   - **Description**: Provides a web-based user interface for monitoring and managing Zookeeper clusters.
   - **Usage**: Enables developers to visualize and manage Zookeeper instances effectively through an intuitive interface.

3. **Kafka**:
   - **Description**: A distributed streaming platform used for real-time data ingestion and processing.
   - **Usage**: Facilitates high-throughput, fault-tolerant ingestion of telemetry data from vehicles in real-time.

4. **Kafka UI**:
   - **Description**: Offers a user interface for monitoring Kafka clusters and topics.
   - **Usage**: Provides developers with insights into Kafka cluster health, topic configuration, and message throughput.

5. **Spark&Flink**:
   - **Description**: Acts as the Spark master node, managing the allocation of resources and scheduling tasks across the Spark cluster.
   - **Usage**: Enables real-time data processing and analytics, including complex event processing and machine learning.

6. **HBase**:
   - **Description**: A distributed NoSQL database that provides real-time read/write access to large datasets.
   - **Usage**: Stores and serves telemetry data in real-time, enabling random, real-time access for analysis and querying.

7. **HDFS** (Hadoop Distributed File System):
   - **Description**: A distributed file system designed to store and manage large volumes of data across multiple machines.
   - **Usage**: Provides scalable and reliable storage for large datasets generated by vehicles, ensuring data durability and accessibility.

8. **MLFlow** (Machine Learning Experiment Tracking Server):
   - **Description**: A tracking server to help MLE's keep track of their experiments, staged, in-production and archived models.
   - **Usage**: Provides storage and insights for ML models in between runs.

9. **MinIO** (High Speed Object Storage):
   - **Description**: An S3 compatible store to manage large volumes of data across multiple machines.
   - **Usage**: Originally used for the MLFlow server to store experiment data, it can be used for anything from simple storage to data lakes.

10. **PostgreSQL** (SQL Database):
   - **Description**: A widely used, fast SQL database for tabular data.
   - **Usage**: Originally used by the MLFlow Tracking Server, you can go ahead and use it to create any other tables you need for other uses.

11. **pgAdmin4**:
   - **Description**: A useful tool for viewing SQL database data.
   - **Usage**: Provides a user friendly UI to help you navigate the PostgreSQL database, view and perform modifications to it.

12. **Grafana**:
   - **Description**: A useful tool for viewing and creating dashboards.
   - **Usage**: The holy grail of dashboards, used for real time dashboards for the car.


### Development Environment Considerations

- **Portability**: Docker containers enable easy replication and deployment of the entire Big Data stack across different development environments.
- **Scalability**: The architecture is designed to scale horizontally, allowing developers to add additional instances or resources as needed for testing and experimentation.
- **Resource Isolation**: Each component runs within its own container, providing isolation and preventing interference between services during development and testing.
- **Simplified Setup**: By encapsulating complex distributed systems within containers, developers can quickly set up and tear down the environment without the need for manual configuration or dependency management.



# Deployment

IMPORTANT NOTE: this stack is PARTIALLY EPHEMERAL. If you're going to launch it and then tear it down, at least for the time being, EXPECT SOME DATA LOSS. I'm slowly working my way to add persistence (HDFS and Grafana are left I think).

# Machine Requirements

At minimum, you need

 - 2vCPU
 - 8GB RAM
 - 48GB disk

# Passwords

You can find all access passwords in the config.env file.

# How to run this stack


## Setup your dev environment with venv

 - Run ```sudo apt-get install python3-virtualenv python3 python3-pip```
 - Run ```virtualenv -p python3 NAMEOFVENVYOUWANT```
 - Run ```source NAMEOFVENVYOUWANT/bin/activate```
 - Run ```pip install scikit-learn mlflow[extras] minio```

## Launch this stack

 - Run ```./launchcommand.sh```

# Remarks
 - If you are running your model training on a host different than the host MLFlow is on, modify the example in the examples/ directory, just search for localhost:5000 and replace with the remote IP but keep the same port.
 - For the image currently being run for kafka, all utilities are in the /opt/kafka/bin/ directory.
 - For the image currently being run for hadoop, hadoop home directory is in /opt/hadoop
 - The current kafka server configuration is setup to automatically register any new topics if they don't exist.

# Sources

 - https://blog.min.io/setting-up-a-development-machine-with-mlflow-and-minio/
 - https://hub.docker.com/r/ubuntu/kafka
 - https://www.mlflow.org/docs/1.20.2/tutorials-and-examples/tutorial.html
 - https://github.com/databricks/mlops-stacks/blob/main/doc-images/mlops-stack-summary.png
 - https://hub.docker.com/r/bitnami/spark
 - https://hub.docker.com/r/apache/spark
 - https://github.com/hyness/hbase-rest-standalone
# Issues

apache-flink won't install for some reason, need to figure it out so i can run the example py file in examples with the flink cluster


# Todo

[x] modified build for MLFlow container

[x] Fixed Kafka advertised listeners issue

[x] Rename files properly (i.e psql_servers, server.properties, config etc) to make things more readable

[x] Parameterize the rest of the stack and add to toolchain-config.env

[x] Add named volumes to Grafana to establish persistence

[x] Add named volumes to HBase to establish persistence

[ ] Connect Flink with HDFS

[ ] Connect Kafka with HDFS
