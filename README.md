# API-to-Kafka-to-PostgreSQL Streaming Pipeline

A notebook-based prototype that demonstrates how intraday market data can move from an external API through Confluent Kafka into a PostgreSQL database hosted on Aiven.

## Architecture

```text
Alpha Vantage API (IBM, 5-minute time series)
        |
        v
Python producer (requests + confluent-kafka)
        |
        v
Confluent Kafka topic: topic_0
        |
        v
Python consumer (confluent-kafka)
        |
        v
PostgreSQL on Aiven (psycopg2)
```

## What is implemented

- Retrieves the IBM intraday time series from Alpha Vantage.
- Serializes timestamped observations as JSON messages.
- Publishes records to a Kafka topic using the Confluent Kafka Python client.
- Consumes Kafka messages and inserts OHLCV fields into PostgreSQL.
- Uses Kafka delivery callbacks and consumer polling.

This is an educational prototype implemented in Jupyter notebooks, not a production-hardened streaming service. It currently has no automated deployment, schema registry, dead-letter queue, retry/backoff policy, observability stack, or automated integration tests.

## Repository

- `PRODUCER.ipynb` — API extraction and Kafka publishing.
- `CONSUMER.ipynb` — Kafka consumption and PostgreSQL persistence.
- `.env.example` — names of required environment variables (placeholders only).

## Requirements

- Python 3.9+
- Confluent Cloud Kafka cluster and API credentials
- Aiven PostgreSQL service
- Alpha Vantage API access (the notebook currently uses the public demo key; replace it with your own key if required)

Install dependencies:

```bash
python -m pip install requests confluent-kafka psycopg2-binary python-dotenv
```

## Configure credentials safely

1. Copy `.env.example` to `.env`.
2. Replace the placeholders with your own service values.
3. Load the variables into your notebook kernel before running the cells. For example, with `python-dotenv`:

```python
from dotenv import load_dotenv
load_dotenv()
```

The notebooks read Kafka settings from `KAFKA_BOOTSTRAP_SERVERS`, `KAFKA_API_KEY`, and `KAFKA_API_SECRET`. The consumer also reads the Aiven connection settings from the `AIVEN_DB_*` variables.

**Never commit `.env`, API keys, passwords, or private service endpoints.** If credentials were previously committed, revoke and rotate them; removing them from the latest version does not remove them from Git history.

## Suggested run order

1. Configure the environment variables.
2. Run `PRODUCER.ipynb` to publish the API observations.
3. Ensure the PostgreSQL target table exists (the consumer notebook includes an example table definition in its comments; verify/create the table before ingestion).
4. Run `CONSUMER.ipynb` to consume and persist messages.

## Next engineering improvements

- Move notebook logic into reusable producer and consumer Python modules.
- Add a versioned event schema and explicit message validation.
- Add idempotent writes and offset/transaction handling.
- Add retry/backoff and dead-letter handling.
- Add structured logging, metrics, and integration tests.
- Provide a reproducible local development environment with Docker Compose.

## Technologies

Python · REST API · Confluent Kafka · PostgreSQL · Aiven · Jupyter
