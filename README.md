# Azure Databricks Streaming Project with Event Hub

End-to-end real-time data engineering pipeline simulating an Uber-like ride confirmation system.
The project demonstrates how to build a modern streaming data platform using the Azure ecosystem
and Databricks with PySpark Structured Streaming.

ARCHITECTURE

Web Application
      │
      ▼
Azure Event Hub (Kafka Streaming)
      │
      ▼
Azure Data Factory (Metadata Ingestion)
      │
      ▼
Azure Data Lake Storage (Bronze Layer)
      │
      ▼
Azure Databricks (PySpark Structured Streaming)
      │
      ▼
Medallion Architecture
      │
      ▼
Star Schema Data Model

DATA FLOW

Web App → Event Hub → Databricks Streaming → Bronze Layer → Silver Layer → Gold Layer

AZURE RESOURCES

Resource Group
 ├── Azure Databricks
 ├── Azure Data Factory
 ├── Azure Event Hub
 ├── Azure Data Lake Storage
 └── Access Connector for Databricks

MEDALLION ARCHITECTURE

Bronze Layer
    Raw streaming data from Event Hub

Silver Layer
    Cleaned and structured data

Gold Layer
    Analytics-ready dimensional model

STREAMING PIPELINE (Databricks)

rides_raw
     │
     ▼
stg_rides
     │
     ▼
silverOneBigTable
     │
     ├── dim_driver
     ├── dim_passenger
     ├── dim_vehicle
     ├── dim_payment
     ├── dim_booking
     ├── dim_location
     └── fact

STAR SCHEMA MODEL

Fact Table
    fact

Dimension Tables
    dim_passenger
    dim_driver
    dim_vehicle
    dim_payment
    dim_booking
    dim_location

KEY FEATURES

- Real-time ride event streaming using Azure Event Hub
- Metadata-driven ingestion using Azure Data Factory
- PySpark Structured Streaming in Azure Databricks
- Medallion architecture (Bronze → Silver → Gold)
- Star schema data modeling
- Slowly Changing Dimensions (SCD Type 2)
- Metadata-driven SQL transformations using Jinja templates

TECHNOLOGIES

Python
PySpark
Spark Structured Streaming
Azure Event Hub
Azure Data Factory
Azure Data Lake Storage (ADLS)
Azure Databricks
Delta Lake
Jinja
GitHub

GITHUB REPOSITORY

https://github.com/ziatily2/Azure-Databricks-Streaming-Project-with-Event-HubAzure Databricks Streaming Project with Event Hub

End-to-end Real-Time Data Engineering Pipeline simulating a ride confirmation system similar to Uber.
The project demonstrates how to build a modern streaming data platform using Azure services and Databricks.

This architecture ingests ride events from a simulated web application, streams them through Azure Event Hub, processes them using PySpark Structured Streaming, and models the data using a Star Schema for analytics.

Architecture

The project implements a real-time data pipeline using Azure cloud services.

Web Application → Event Hub → Azure Data Factory → ADLS → Databricks Streaming → Star Schema


Producer (Web App)
        │
        ▼
Azure Event Hub (Kafka Streaming)
        │
        ▼
Azure Data Factory (Metadata Ingestion)
        │
        ▼
Azure Data Lake Storage (Bronze Layer)
        │
        ▼
Azure Databricks + PySpark Streaming
        │
        ▼
Medallion Architecture (Bronze → Silver → Gold)
        │
        ▼
Star Schema Data Model



Project Components
1 Web Application

A Python-based web application simulates ride booking events.

Each event contains information such as:

ride_id

passenger

driver

vehicle

pickup/dropoff location

payment method

fare information

timestamps

These events are sent to Azure Event Hub in real time.

Azure Services Used
| Service                 | Purpose                                     |
| ----------------------- | ------------------------------------------- |
| Azure Event Hub         | Real-time event streaming platform          |
| Azure Data Factory      | Metadata-driven pipeline orchestration      |
| Azure Data Lake Storage | Data lake for Bronze/Silver/Gold layers     |
| Azure Databricks        | Streaming data processing using Spark       |
| GitHub                  | Storage for metadata files and project code |



Event Streaming

Ride confirmation events are streamed into Azure Event Hub.

Example event:
{
  "ride_id": "a60d8f76",
  "passenger_id": "p102",
  "driver_id": "d21",
  "vehicle_id": "v54",
  "pickup_city_id": 3,
  "payment_method_id": 2,
  "distance_miles": 4.2,
  "duration_minutes": 12,
  "total_fare": 18.5
}


Event Hub acts as a Kafka-compatible streaming platform.

Data Ingestion (Azure Data Factory)

Azure Data Factory is used to ingest metadata mapping files from GitHub.

Mapping files include:

cities

vehicle types

payment methods

ride status

cancellation reasons

The pipeline dynamically fetches metadata using the GitHub API and stores it in ADLS Bronze Layer.

ADF pipeline uses:

Lookup Activity

ForEach Activity

Copy Activity

This creates a metadata-driven ingestion framework.

Databricks Streaming Processing

Databricks processes streaming data using PySpark Structured Streaming.

Event Hub is consumed using Kafka protocol.

Example configuration:
KAFKA_OPTIONS = {
    "kafka.bootstrap.servers": "namespace.servicebus.windows.net:9093",
    "subscribe": "ilyas_streaming",
    "kafka.security.protocol": "SASL_SSL",
    "kafka.sasl.mechanism": "PLAIN",
}


Streaming pipeline reads events continuously and stores them in Delta tables.

Medallion Architecture

The project follows a Medallion architecture:

Bronze Layer

Raw streaming data from Event Hub.

Tables:

rides_raw

mapping tables

Silver Layer

Cleaned and structured data.

Transformations include:

JSON parsing

schema enforcement

data normalization

Tables:

stg_rides

silverOneBigTable

Gold Layer

Analytics-ready tables modeled as a Star Schema.

Star Schema Model

The Gold layer implements a dimensional model.

Fact Table
fact
Contains ride-level metrics.

Columns include:

ride_id

driver_id

passenger_id

vehicle_id

payment_method_id

pickup_city_id

duration_minutes

distance_miles

total_fare


Dimension Tables
dim_passenger
dim_driver
dim_vehicle
dim_payment
dim_booking
dim_location


Each dimension tracks entity attributes.

Slowly Changing Dimensions (SCD Type 2)

Dimension tables implement SCD Type 2 to maintain history.

This tracks changes such as:

driver information updates

passenger profile updates

vehicle updates

Using Databricks Declarative Pipelines.

Metadata Driven Transformations

The project uses Jinja templating to dynamically generate SQL transformations.

Example template:
SELECT
{% for config in jinja_config %}
    {{ config.select }}
{% endfor %}
FROM
{% for config in jinja_config %}
    {% if loop.first %}
        {{ config.table }}
    {% else %}
        LEFT JOIN {{ config.table }} ON {{ config.on }}
    {% endif %}
{% endfor %}


This allows transformations to be controlled via metadata instead of hardcoded SQL.

Databricks Pipeline Graph

The Databricks pipeline orchestrates:
rides_raw
     │
     ▼
stg_rides
     │
     ▼
silverOneBigTable
     │
     ├── dim_driver
     ├── dim_passenger
     ├── dim_vehicle
     ├── dim_payment
     ├── dim_location
     └── fact




Technologies Used

Python
PySpark
Spark Structured Streaming
Azure Event Hub
Azure Data Factory
Azure Data Lake Storage (ADLS)
Azure Databricks
Delta Lake
Jinja
GitHub

Key Concepts Demonstrated

Real-time streaming pipelines
Medallion architecture
Metadata-driven pipelines
Event streaming with Kafka-compatible systems
Slowly Changing Dimensions (SCD Type 2)
Dimensional modeling (Star Schema)
Data orchestration using Azure Data Factory

Future Improvements

Add monitoring and alerting

Implement CI/CD with GitHub Actions

Add dashboard layer (Power BI)

Deploy infrastructure using Terraform

Author

Ilyas

Data Engineering Project
Azure + Databricks + Streaming


