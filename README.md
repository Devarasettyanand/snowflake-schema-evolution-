# snowflake-schema-evolution
This project demonstrates a real-time Schema Evolution pipeline in Snowflake, designed to replicate how large enterprises (such as Kroger, Walmart, Target, and Amazon Fresh) ingest and manage continuously changing JSON data structures in production.

Modern retail and e-commerce platforms generate millions of transactions daily, and the schema of these payloads evolves frequently due to:

New payment methods

New promotional attributes

New metadata fields

API version changes

Additional nested objects

Type drift between versions (e.g., number → string)

Traditional ETL systems break when schemas change.
Snowflake’s native schema evolution and schema inference capabilities solve this seamlessly.

🎯 What This Project Shows

This repository walks step-by-step through:

✔ Creating a storage integration to access S3

✔ Creating an external stage to read JSON files

✔ Automatically inferring schema from JSON using INFER_SCHEMA

✔ Auto-creating a table using USING TEMPLATE

✔ Loading the first dataset (v1) with a stable schema

✔ Enabling schema evolution on the target table

✔ Loading a second dataset (v2) with a changed schema

✔ Snowflake automatically:

Detects new columns

Merges new schema into the existing table

Handles type drift safely

Preserves original data

🧪 Files Used in This Project

transactions_v1.json

Flat schema (transaction, user, payment, timestamp).

transactions_v2.json

Evolved schema including:

coupon_code (new field)

metadata object with nested values

Type drift (amount: numeric → string)

This allows testing enterprise-grade schema drift scenarios.

🏗 Architecture Summary

JSON files land in an S3 bucket.

Snowflake reads the file metadata via an external stage.

Schema is inferred from the raw JSON.

Snowflake auto-creates the table structure.

Data is loaded with COPY INTO.

When new files with additional fields arrive,

Snowflake seamlessly merges the schema without breaking the pipeline.

🏢 Why This Project Is Realistic for Enterprise

This setup directly reflects how companies manage raw API ingestion, POS logs, mobile app events, and customer activity streams that evolve frequently.
Engineering teams rely heavily on:

INFER_SCHEMA

USING TEMPLATE

ENABLE_SCHEMA_EVOLUTION

External stages

Flexible VARIANT-based modeling

This repository serves as a starter real-time ingestion pipeline that can be expanded into:

Snowpipe auto-ingestion

Schema registry logging

dbt transformations (bronze → silver → gold)

Drift alerts

Full CDC pipeline

🚀 Who Is This For?

This project is ideal for:

Beginners building their first real-time Snowflake pipeline

Data engineers preparing for Snowflake interviews

Professionals building a portfolio project

Anyone learning schema evolution, ingestion, VARIANT modeling, or JSON processing

✨ Outcome

By completing this project, you will:

Understand how Snowflake handles schema drift

Build a fully functional ingestion pipeline

Create a production-ready GitHub repository

Gain hands-on, job-relevant Snowflake experience
