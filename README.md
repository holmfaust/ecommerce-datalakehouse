# E-Commerce Data Lakehouse

This project demonstrates an end-to-end data lakehouse pipeline for E-Commerce analytics using **Apache Spark**, **Delta Lake**, **PostgreSQL**, and **Jupyter**. The architecture follows the **Medallion Architecture** (Bronze ➔ Silver ➔ Gold).

## Architecture & Data Flow

1. **Source**: Raw E-Commerce data ingested from a PostgreSQL database (`ecommerce_source`).
2. **Bronze Layer**: Raw data ingestion from the source system (stored in Delta format).
3. **Silver Layer**: Data cleaning, filtering, and joining entities.
4. **Gold Layer**: Business-level aggregations and business logic. It includes **Slowly Changing Dimensions (SCD Type 2)** implementation for tracking historical customer/product changes.
5. **Analytics**: Optimization (Z-Ordering, OPTIMIZE) and analytical querying on the Gold data.

## Tech Stack

- **Apache Spark (3.5.0)** & **PySpark**: Distributed data processing.
- **Delta Lake**: Open-source storage framework that enables building a Lakehouse architecture.
- **PostgreSQL (15)**: Relational database representing the operational source database.
- **Docker & Docker Compose**: Containerized environment for local development (Spark Master, Spark Worker, PostgreSQL, JupyterLab).
- **JupyterLab**: Interactive development environment for PySpark notebooks.

## Directory Structure
```
.
├── notebooks/
│   ├── 01_bronze_to_silver.ipynb      # Ingestion (Source -> Bronze) & Transformation (Bronze -> Silver)
│   ├── 02_silver_to_gold_scd2.ipynb   # SCD Type 2 logic and business aggregations (Silver -> Gold)
│   └── 03_optimize_and_analytics.ipynb # Delta Lake Optimization and analytical queries
├── scripts/
│   └── load_postgres.py               # Script to load initial mock/source data into Postgres
├── data/                              # Local volume for Bronze, Silver, Gold Delta tables
├── docker-compose.yml                 # Local environment infrastructure
├── requirements.txt                   # Python dependencies
└── .gitignore                         # Project git ignores (notebook checkpoints, venv, data tables)
```

## Getting Started

1. **Start the Infrastructure**
   Start the Spark cluster, PostgreSQL instance, and JupyterLab environment:
   ```bash
   docker-compose up -d
   ```

2. **Access the Services**
   - **JupyterLab**: [http://localhost:8888](http://localhost:8888)
   - **Spark UI**: [http://localhost:8080](http://localhost:8080)
   - **PostgreSQL**: `localhost:5432` (Username: `admin`, Password: `password123`, DB: `ecommerce_source`)

3. **Run the Pipeline**
   Open the `notebooks/` folder in JupyterLab and execute the notebooks in order:
   1. `01_bronze_to_silver.ipynb`
   2. `02_silver_to_gold_scd2.ipynb`
   3. `03_optimize_and_analytics.ipynb`

## Cleanup

To stop and remove all running containers and networks:
```bash
docker-compose down
```
If you also want to remove locally mapped data volumes, simply delete the `data/bronze/`, `data/silver/`, and `data/gold/` directories.
