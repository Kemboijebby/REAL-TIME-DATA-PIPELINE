Real-Time Data Pipeline

This repository contains a real-time data pipeline that fetches data from an API, processes it, and stores it in a PostgreSQL database hosted on Aiven. The pipeline utilizes Confluent for data streaming.

Overview

The pipeline consists of the following components:

API Producer: Fetches data from a specified API endpoint.

Kafka Producer: Sends the fetched data to a Kafka topic on Confluent.

Kafka Consumer: Consumes data from the Kafka topic.

PostgreSQL Storage: Stores the consumed data in a PostgreSQL database hosted on Aiven.

Prerequisites


Python 3.7 or higher

Confluent Kafka

PostgreSQL on Aiven
