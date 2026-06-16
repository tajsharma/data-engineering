# Data Engineering Zoomcamp

My work from the [DataTalks.Club Data Engineering Zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp) — a free 9-week course on building end-to-end, production-style data pipelines.

This repository contains the pipelines, infrastructure code, and homework I built while working through the course. Each module folder is self-contained with its own README documenting what I built, how to run it, and the answers to that module's homework.

> 📚 Course reference: [DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

## Tech stack

| Area | Tools |
| --- | --- |
| Containerization & IaC | Docker, Docker Compose, Terraform |
| Cloud | Google Cloud Platform (GCS, BigQuery) |
| Workflow orchestration | Kestra |
| Data ingestion | dlt |
| Transformation / analytics engineering | dbt, DuckDB |
| Data platform | Bruin |
| Batch processing | Apache Spark |
| Streaming | Apache Kafka |
| Languages | Python, SQL |

---

## Progress

| # | Module | Focus | Status |
| --- | --- | --- | --- |
| 01 | [Docker & Terraform](./01-docker-terraform) | Containerization, PostgreSQL, GCP, Infrastructure as Code | 🔨 In progress |
| — | [Workshop 1: Data Ingestion](./workshops/dlt) | Building scalable ingestion pipelines with dlt | ⬜ Not started |
| 02 | [Workflow Orchestration](./02-workflow-orchestration) | Pipelines, scheduling, and backfills with Kestra | ⬜ Not started |
| 03 | [Data Warehousing](./03-data-warehouse) | BigQuery, partitioning, and clustering | ⬜ Not started |
| 04 | [Analytics Engineering](./04-analytics-engineering) | Data modeling with dbt (DuckDB & BigQuery) | ⬜ Not started |
| 05 | [Data Platforms](./05-data-platforms) | End-to-end pipelines with Bruin | ⬜ Not started |
| 06 | [Batch Processing](./06-batch) | Apache Spark DataFrames & SQL | ⬜ Not started |
| 07 | [Streaming](./07-streaming) | Kafka, Kafka Streams, and schema management | ⬜ Not started |
| — | [Final Project](./project) | End-to-end pipeline applying every concept | ⬜ Not started |

✅ Complete &nbsp;·&nbsp; 🔨 In progress &nbsp;·&nbsp; ⬜ Not started

---

## Repository structure

```
de-zoomcamp/
├── README.md                     ← you are here
├── 01-docker-terraform/
│   ├── README.md                 ← what I built + homework answers
│   ├── docker-compose.yaml
│   ├── Dockerfile
│   ├── ingest_data.py
│   ├── pyproject.toml
│   └── terraform/
├── 02-workflow-orchestration/
├── 03-data-warehouse/
├── 04-analytics-engineering/
├── 05-data-platforms/
├── 06-batch/
├── 07-streaming/
├── workshops/
└── project/
```

Each module is independent. To run a given module, open its folder and follow the README there.

---

## About

Built by **Taj Sharma** while working through the Data Engineering Zoomcamp, as part of an ongoing focus on data engineering and analytics engineering.

🔗 Portfolio: [tajsharma.com](https://tajsharma.com)
