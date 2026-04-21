# Snowflake Data Loading – Big Picture

Think of Snowflake data movement in 3 layers:

**Source → Stage → Load → Transform → Query → Unload**

## 1. Data Ingestion Methods (When to Use What)

| Method | Use Case | Key Feature |
| --- | --- | --- |
| `INSERT` | Small or manual data | Simple SQL |
| `COPY INTO <table>` | Bulk loading | Fast, scalable |
| Snowpipe | Real-time ingestion | Serverless, auto-load |

**Rule of thumb:**  
- Small → `INSERT`  
- Large batch → `COPY`  
- Streaming → Snowpipe  

## 2. Stage = Heart of Data Movement

Stages are temporary file storage locations.

| Type | Example | Use Case |
| --- | --- | --- |
| User Stage | `@~` | Personal quick loads |
| Table Stage | `@%table` | Table-specific |
| Named Stage | `@stage_name` | Production pipelines |
| External Stage | S3, Azure, GCS | Enterprise pipelines |

**Best Practice:**  
Use Named Stage + Storage Integration

## 3. Bulk Loading (Core Concept)

`COPY INTO <table>`  
Loads data from Stage → Table  
Uses Virtual Warehouse  

**Supports:**  
- CSV, JSON, Parquet, etc.  
- Transformations during load  

**Key Options:**  
- `ON_ERROR` → control failures  
- `PURGE` → delete files after load  
- `FORCE` → reload duplicates  

**Important Behavior:**  
- Deduplication based on file name + content  
- Load history retained 64 days  

## 4. Snowpipe (Real-Time)

**How it works:**  
File arrives → Event trigger → Snowpipe → Table  

**Key Points:**  
- Serverless (no warehouse)  
- Auto ingestion (~1 min latency)  
- Load history: 14 days  

**When to use:**  
- Streaming data  
- Frequent small files  

## 5. File Format = Parsing Brain

Defines how Snowflake reads files.

**Options:**  
- CSV → delimiter, header  
- JSON → structure  
- Compression → GZIP, AUTO  

**Best Practice:**  
Always use `FILE FORMAT` object (reusable)

## 6. Data Unloading (Egress)

`COPY INTO <location>`  
Table → Stage or Cloud storage  

**Supports:**  
- CSV, JSON, Parquet  

**Features:**  
- Partitioning  
- Query-based export  
- Multiple output files  

**GET Command:**  
Internal Stage → Local system  

## 7. Semi-Structured Data (Key Differentiator)

| Type | Purpose |
| --- | --- |
| `VARIANT` | Store JSON |
| `ARRAY` | List |
| `OBJECT` | Key-value |

**Most used:** `VARIANT`  

## 8. ELT vs ETL (Very Important for Interviews)

| Approach | Description | When to Use |
| --- | --- | --- |
| ELT | Load raw JSON → transform later | Unknown schema |
| ETL | Transform during load | Performance critical |

## 9. Querying JSON

**Dot Notation:**  
`column:key.subkey`  

**Array Access:**  
`column:key[0]`  

**Important:**  
- JSON keys are case-sensitive  
- Always `CAST` values  

## 10. FLATTEN (Most Asked Topic)

**Purpose:**  
Convert nested arrays → rows  

`ARRAY` → multiple rows  
With `LATERAL`:  
Keeps relation with original table  

**Use case:**  
Normalize JSON for analytics  

## 11. End-to-End Architecture (Interview Gold)

Source System / Kafka / API
↓
Cloud Storage (S3 / Azure Blob)
↓
External Stage
↓
Snowpipe (real-time) OR COPY INTO (batch)
↓
Raw Table (VARIANT - ELT)
↓
Transform (SQL / Streams / Tasks)
↓
Curated Tables
↓
BI Tools (Power BI / Tableau)


## Final Key Takeaways

- **Stage** = central component  
- **COPY INTO** = backbone of ingestion  
- **Snowpipe** = real-time automation  
- **VARIANT** = semi-structured power  
- **FLATTEN** = must-know function  
