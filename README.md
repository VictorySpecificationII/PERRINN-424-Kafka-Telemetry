# Solution Design

## Checkpoint 1

Unity -> kafka -> spark -> cassandra

## Checkpoint 2

Unity -> kafka -> flink -> cassandra

## Introduction

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

## Component Overview

 To be completed when stack is complete and stable.


## Development Environment Considerations

- **Portability**: Docker containers enable easy replication and deployment of the entire Big Data stack across different development environments.
- **Scalability**: The architecture is designed to scale horizontally, allowing developers to add additional instances or resources as needed for testing and experimentation.
- **Resource Isolation**: Each component runs within its own container, providing isolation and preventing interference between services during development and testing.
- **Simplified Setup**: By encapsulating complex distributed systems within containers, developers can quickly set up and tear down the environment without the need for manual configuration or dependency management.

# Deployment

IMPORTANT NOTE: this stack is PARTIALLY EPHEMERAL. If you're going to launch it and then tear it down, at least for the time being, EXPECT SOME DATA LOSS. I'm slowly working my way to add persistence (HDFS and Grafana are left I think).

## Machine Requirements

At minimum, you need

 - 4vCPU
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
 - HADOOP CURRENTLY DISABLED: For the image currently being run for hadoop, hadoop home directory is in /opt/hadoop
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

[ ] Connect Flink with S3

[ ] Connect Kafka with S3

[ ] Refactor MinIO S3 service in dockerfile

[ ] Add versions to toolchain config for docker services

[ ] Add Apache Hive for data warehousing

[x] Add Airflow to stack

[ ] Add a service panel (heimdall, homepage?) as a central service directory

[ ] Look into SRE using ELK

[ ] Create a CI/CD pipeline for code

[ ] Look into MLOps
