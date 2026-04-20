Course Detail : https://capgemini.udemy.com/course/ultimate-snowpro-core-certification-course-exam/learn/lecture/34479200#overview

<img width="1385" height="914" alt="image" src="https://github.com/user-attachments/assets/9407b58d-f99d-4572-a361-bcac2280a4a4" />
<img width="1335" height="934" alt="image" src="https://github.com/user-attachments/assets/abeede5f-c9cb-4a28-bcd9-b1dfb13d5ccc" />
<img width="1554" height="947" alt="image" src="https://github.com/user-attachments/assets/0d18d8a4-bbf6-4d8f-afb0-f9436b573082" />


<details> <summary>Snowflake – Concise Summary</summary>

Snowflake is a **cloud-native, SaaS-based data platform** (AI Data Cloud) that extends beyond a traditional data warehouse.

### Key Points

- **Unified Platform**  
  Combines capabilities of a **data warehouse, data lake, and database**.

- **Multi-Language Support**  
  Primarily SQL-based, with support for **Python, Java, Scala**.

- **End-to-End Data Lifecycle**  
  Handles **data ingestion, transformation, analytics, governance, and app development**.

- **Advanced Features**  
  - ACID-compliant transactions  
  - Built-in security (encryption, RBAC, MFA)  
  - Semi-structured data support (e.g., JSON)  
  - High concurrency and scalability  

- **AI & ML Capabilities**  
  Includes **Cortex (LLMs), Snowpark, Streamlit**, enabling ML and AI workflows.

- **Cloud-Native Architecture**  
  Built from scratch for the cloud (runs on **AWS, Azure, GCP**) with:  
  - Scalability  
  - Elasticity  
  - High availability  

- **SaaS Model**  
  - No infrastructure management  
  - Automatic updates and optimization  
  - **Pay-as-you-go pricing**




### Summary

> Snowflake is an all-in-one, cloud-native platform for **data storage, processing, analytics, and AI**, with minimal operational overhead.

</details>

<details> <summary>Snowflake Architecture</summary> 

  ### Traditional Architectures

#### 1. Shared-Disk Architecture
- Multiple compute nodes share **centralized storage**
- **Pros:**
  - Simple management
  - Single source of truth
- **Cons:**
  - Single point of failure
  - Network/bandwidth bottlenecks
  - Limited scalability
  - Resource contention

#### 2. Shared-Nothing Architecture
- Each node has **its own compute + storage**
- Used in systems like Hadoop/Spark
- **Pros:**
  - Better performance (local data access)
  - Scalable and cost-effective
- **Cons:**
  - Data distribution challenges
  - Expensive data shuffling
  - Tight coupling of compute & storage
  - Over-provisioning and resource contention

---

### Snowflake Architecture  
**Multi-Cluster Shared Data Architecture**

A **cloud-native, service-oriented architecture** with 3 layers:

#### 1. Storage Layer
- Centralized cloud storage
- Stores all data (tables, databases)
- Similar to shared-disk model

#### 2. Compute Layer (Virtual Warehouses)
- Independent compute clusters
- Execute queries
- Can cache data locally
- Similar to shared-nothing model

#### 3. Cloud Services Layer
- Manages:
  - Authentication
  - Query parsing & optimization
  - Metadata & infrastructure

---

### Key Advantages

- **Decoupled Architecture**
  - Storage, compute, and services scale independently

- **Unlimited Scalability**
  - Near-infinite scaling of compute and storage

- **Multi-Cluster Support**
  - Multiple virtual warehouses
  - Enables **workload isolation** (no resource contention)

- **Cloud-Native Design**
  - Built for AWS, Azure, GCP
  - High availability, elasticity

---

### Key Insight

> Snowflake combines the **best of shared-disk and shared-nothing architectures** while eliminating their limitations through **decoupled, cloud-native design**.

</details>

<details> <summary>Snowflake Storage Layer</summary> 

  ### Overview
- Uses **cloud blob storage** (e.g., AWS S3, Azure Blob, GCS)
- Provides **high scalability, availability, and durability**
- Acts as a **centralized single source of truth**

---

### Data Organization
- Data is structured as:
  - **Databases → Schemas → Tables**
- Supports:
  - **Structured data** (CSV, TSV)
  - **Semi-structured data** (JSON, Avro, Parquet)

---

### Internal Storage Format
- Data is converted into a **proprietary columnar format**
- **Benefits:**
  - Faster query performance (read optimization)
  - Only required columns are scanned

---

### Optimization Features (Automatic)
- **Compression**
  - Reduces storage size
  - More efficient with columnar data

- **Encryption**
  - Default **AES-256 encryption** (at rest)

- **Micro-Partitioning**
  - Data split into small partitions
  - Enables **query pruning** (skip irrelevant data)

---

### Key Characteristics
- Fully **managed by Snowflake** (no user intervention)
- Data **not directly accessible** in storage (only via SQL)
- Inherits cloud features:
  - High availability
  - Data replication across zones
  - Durability guarantees

---

### Billing
- Charged **per TB of data stored**
- Calculated monthly

---

### Summary

> Snowflake’s storage layer is a **fully managed, scalable, and secure cloud storage system** optimized with columnar format, compression, encryption, and micro-partitions for high-performance analytics.
</details>

<details> <summary>Snowflake Query Processing Layer (Compute Layer)</summary> 

  ### Overview
- Responsible for **executing queries** on data from the storage layer  
- Built using **virtual warehouses** (Snowflake-managed compute clusters)

---

### Virtual Warehouses
- Logical abstraction of **cloud compute clusters** (e.g., AWS EC2)
- Created/managed via **SQL or UI**
- Execute query plans generated by the services layer
- Users interact only with the **warehouse object**, not underlying infrastructure

---

### Key Characteristics

#### 1. Shared-Nothing Behavior
- Each warehouse:
  - Fetches data from storage
  - Uses **local cache** for faster subsequent queries

#### 2. Ephemeral & Flexible
- Can be:
  - **Created / Dropped instantly**
  - **Paused / Resumed**
- **No cost when paused** → cost optimization

#### 3. Scalability
- **Scale Up**: Increase warehouse size (XS → 6XL)
- **Scale Out**: Create multiple warehouses
- Supports **high concurrency workloads**

#### 4. Workload Isolation
- Each warehouse is **independent**
- Example:
  - One for **data ingestion**
  - One for **analytics**
- Prevents **resource contention**

#### 5. Configurable
- Each warehouse can have:
  - Different **size**
  - Different **settings** based on workload

---

### Data Access & Consistency
- All warehouses access the **same centralized storage**
- Ensures **ACID compliance (strong consistency)**
- Managed by **Transaction Manager** (services layer)

---

### Cost Optimization
- Pay only when warehouse is **running**
- Pause during idle periods to save cost

---

### Summary

> Snowflake’s compute layer uses **virtual warehouses** to provide scalable, isolated, and cost-efficient query processing with strong consistency and high concurrency.
</details>

<details> <summary>Snowflake Services Layer (Cloud Services Layer) </summary> 

### Overview
- A **collection of highly available, scalable services**
- Coordinates all activities across Snowflake
- Also called the **Global Services Layer**
- Fully managed (no user control or visibility)

---

### Key Responsibilities

#### 1. Authentication & Access Control
- Verifies user identity (login)
- Manages **roles and permissions**

#### 2. Infrastructure Management
- Provisions and manages:
  - Compute (virtual warehouses)
  - Storage (cloud blob storage)

#### 3. Transaction Management
- Ensures **ACID compliance**
- Maintains **data consistency** across all warehouses

#### 4. Metadata Management
- Stores:
  - Table/schema definitions
  - Data statistics
- Uses a **scalable key-value store (metadata cache)**
- Critical for **query performance optimization**

#### 5. Query Parsing & Optimization
- Converts SQL into an **execution plan**
- Optimizes queries before execution

#### 6. Security Services
- Handles:
  - Encryption
  - Key management & rotation

---

### Key Characteristics

- **Global Multi-Tenant Layer**
  - Shared across all Snowflake accounts
  - Enables features like **secure data sharing**

- **Highly Scalable & Available**
  - Runs on cloud compute (managed by Snowflake)

- **Invisible to Users**
  - No direct interaction or configuration required

---

### Summary

> The services layer is the **brain of Snowflake**, managing authentication, metadata, query optimization, transactions, and infrastructure—ensuring everything runs efficiently and securely behind the scenes.
</details>

<details> <summary>Snowflake Object Model </summary> 

### 🧊 Account
- A **Snowflake account** is an isolated environment with compute, storage, and services.
- Tied to:
  - **One cloud provider**: AWS, GCP, or Azure
  - **One region**: impacts cost, latency, compliance, and features
- Accessible via a **unique URL** (org + account name).
- Contains **system-defined roles** (e.g., `ACCOUNTADMIN`).
- Edition can be changed after creation.

---

### 🏢 Organization
- A **metadata container** for managing one or more accounts.
- Types:
  - **Single-account organization**
  - **Multi-account organization** (enterprise use)
- Key roles:
  - `ORGADMIN` → create/manage organization account
  - `GLOBALORGADMIN` → manage all accounts
- Capabilities:
  - Create/delete/rename accounts (`CREATE ACCOUNT`, `SHOW ACCOUNTS`)
  - Monitor **usage & billing** across accounts
  - Enable **replication & failover**

---

### 🌐 Account Identifier & URL
- Format: `<org_name>-<account_name>`
- Used in connectors (SnowSQL, Python, etc.)
- UI (Snowsight) uses a different URL format post-login
- Names:
  - Auto-generated (trial)
  - Custom (via Snowflake support)

---

### 🗄️ Database
- **Primary logical container** for data
- Contains **schemas**
- Rules:
  - Unique within an account
  - Must start with a letter
  - No spaces/special chars (unless quoted)
  - Case-sensitive if quoted
- Features:
  - **Clone**, **replicate**, or **create from share**

---

### 📂 Schema
- Logical grouping of objects (tables, views, stages)
- Belongs to a **database**
- Rules:
  - Unique within a database
  - Same naming & case rules as databases
- Supports **cloning**

---

### 🧭 Namespace
- Format: `<database>.<schema>`
- Used to reference objects globally
- Can set context using:
```sql
  USE DATABASE db_name;
  USE SCHEMA schema_name;

```

Snowflake uses a hierarchical structure:

- Organization → Account → Database → Schema → Objects
- Region + cloud provider choices impact performance, cost, compliance
- Organizations enable centralized multi-account governance
- Databases and schemas define data organization and access context
  
</details>

<details> <summary>Snowflake Table Types</summary>

### 📊 Overview
- Tables are **logical abstractions** over stored data.
- Define **structure for querying**.
- Key difference across types: **data retention & recovery (Time Travel + Fail-safe)**.

---

### 🧱 1. Permanent Tables (Default)
- Persist **until explicitly dropped**
- **Time Travel**:
  - Up to **90 days** (Enterprise+)
  - **1 day** (Standard)
- **Fail-safe**: **7 days (fixed)**
- ✅ Best for critical, long-term data

---

### ⏳ 2. Temporary Tables
- Exist only for the **session duration**
- Automatically **deleted after session ends**
- **Time Travel**: Max **1 day**
- **Fail-safe**: ❌ Not available
- ❌ Cannot convert to other table types
- ✅ Ideal for **ETL intermediate steps / temp processing**

---

### ⚡ 3. Transient Tables
- Persist until **explicitly dropped**
- **Time Travel**: Max **1 day**
- **Fail-safe**: ❌ Not available
- 💰 Lower storage cost (no fail-safe storage)
- ⚠️ Limited recovery capability
- ✅ Good for **non-critical but reusable data**

---

### 🌐 4. External Tables
- Query data stored **outside Snowflake** (e.g., S3)
- Store only **metadata**, not actual data
- **Read-only**
- **Time Travel**: ❌ Not supported
- **Fail-safe**: ❌ Not supported
- 🐢 Slower performance vs internal tables
- ✅ Useful for **data lake integration**

---

### 🔑 Key Takeaways
- **Permanent** = highest protection (TT + Fail-safe)
- **Temporary** = session-based, no recovery
- **Transient** = cost-optimized, limited recovery
- **External** = query external data, no storage in Snowflake

</details>

<details> <summary>Open Table Formats & Iceberg in Snowflake</summary>

### 🌊 Problem with Data Lakes
- Raw files in object storage (S3, GCS, Blob) lack:
  - **Consistency & concurrency control**
  - **Schema enforcement**
- Multiple tools accessing same data → risk of **conflicts & inconsistency**

---

### 🧩 Open Table Formats (Solution)
- Add a **metadata layer** between data files & query engines
- Provide:
  - Structured **table abstraction**
  - Safe **multi-engine access**

#### 🔑 Popular Formats
- Delta Lake
- Apache Hudi
- **Apache Iceberg** (Snowflake focus)

---

### 🧊 Apache Iceberg Architecture
- Stored in **object storage** under a table root:
  - `/metadata` → versioned JSON (schema, snapshots)
  - `/data` → actual data files (typically **Parquet**)
- **Snapshots**:
  - Represent table state at a point in time
  - Enable versioning & rollback

---

### 📚 Catalog (Key Component)
- Acts as a **table registry**
- Points to latest metadata
- Flow:

<img width="1681" height="737" alt="image" src="https://github.com/user-attachments/assets/bfa191e7-91c1-4400-b74c-b88fca2867cd" />

</details>




<details> <summary>Snowflake Stored Procedures </summary>

### ⚙️ What are Stored Procedures?
- Named collections of **SQL statements + procedural logic**
- Used to **automate and modularize repetitive tasks**
- Example: batch deletes, maintenance jobs

---

## 🧊 Stored Procedures in Snowflake

### 🛠️ Implementation Methods
- **JavaScript** (most established)
- **Snowflake Scripting (SQL + procedural logic)**
- **Snowpark** (Python, Java, Scala)

---

### 📦 Key Characteristics
- Created as **database objects** (within database & schema)
- Can take **0 or more input parameters** (signature)
- Must define a **return type** (even if not used)
- Typically used for **actions**, not returning values

---

### 🔁 Execution
- Invoked using:

CALL procedure_name(...);


Called as a standalone statement
## 🧠 JavaScript Stored Procedures

### ✨ Features
- Mix **JavaScript + SQL**
- Supports:
  - Variables
  - Loops
  - Conditional logic
  - Error handling

---

### 🔗 SQL Execution
- SQL is **built dynamically** within JavaScript
- Executed using **Snowflake JavaScript API**


var sql_command = "SELECT * FROM my_table";
snowflake.execute({ sqlText: sql_command });

## ⚖️ Stored Procedures vs UDFs

| Feature              | Stored Procedure                  | UDF                          |
|----------------------|----------------------------------|------------------------------|
| Usage               | Standalone (`CALL`)              | Inside SQL queries           |
| Return value        | Optional                         | Mandatory                    |
| Return type         | Scalar (JS), Tabular (SQL)       | Scalar / Tabular             |
| Purpose             | Perform actions (DML, admin)     | Compute & return values      |
| Language flexibility| JS, SQL, Snowpark                | Limited (no JS API mixing)   |
| Recursion           | Supported                        | Limited support              |
| SQL integration     | Not usable inline                | Usable in `SELECT`, `WHERE`  |

</details>





<details> <summary>Snowflake Core Objects Reference</summary>
  
#### **1. User-Defined Function (UDF)**
A UDF returns a single scalar value or a tabular result for each row of input. It’s used inside SQL like a built-in function.

| **Attribute** | **Details** |
| --- | --- |
| **Purpose** | Encapsulate reusable logic that returns a value. Used in `SELECT`, `WHERE`, `ORDER BY`, etc. |
| **Languages** | SQL, JavaScript, Java, Python, Scala |
| **Types** | 1. **Scalar UDF**: Returns 1 value per row 2. **Table UDF/UDTF**: Returns a table per row. Call with `TABLE(function())` |
| **State** | Stateless. Cannot run DDL/DML. No side effects. |
| **Execution** | Runs in the context of the calling query. Parallelized per row/partition. |
| **Overloading** | Supported. You can define multiple UDFs with same name but different arg types. |


**Key limit**: Cannot access tables or run queries inside a SQL UDF. Java/Python UDFs can use Snowpark APIs to query data.

#### **2. Stored Procedure**
A procedure executes procedural logic and can run multiple SQL statements, including DDL/DML. Called with `CALL`.

| **Attribute** | **Details** |
| --- | --- |
| **Purpose** | Orchestrate workflows: ETL steps, transactions, loops, error handling, dynamic SQL |
| **Languages** | SQL Scripting, JavaScript, Java, Python, Scala |
| **Return** | Single scalar value: `VARCHAR`, `VARIANT`, etc. Not a result set, unless you use `TABLE()` return type |
| **State** | Can be stateful. Supports transactions, variables, exceptions. |
| **Execution** | Runs as owner or caller: `EXECUTE AS OWNER` or `EXECUTE AS CALLER` |
| **Side Effects** | Can run DDL/DML, create tables, insert data, send emails via external functions |
| **Example** | ```sql
CREATE OR REPLACE PROCEDURE load_and_clean(source_tbl STRING, target_tbl STRING)
RETURNS STRING
LANGUAGE SQL
AS
$$
BEGIN
  DELETE FROM IDENTIFIER(:target_tbl);
  INSERT INTO IDENTIFIER(:target_tbl)
    SELECT DISTINCT * FROM IDENTIFIER(:source_tbl) WHERE value IS NOT NULL;
  RETURN 'Loaded ' || SQLROWCOUNT || ' rows';
END;
$$;

CALL load_and_clean('RAW_DATA', 'CLEAN_DATA');

**UDF vs Stored Procedure**: UDF = expression in SQL, returns per row, no DML. Procedure = called independently, can run DML, returns once.


#### **3. External Function**
Calls code that runs outside Snowflake via an HTTPS proxy service like AWS Lambda, Azure Functions, or GCP Cloud Functions.

| **Attribute** | **Details** |
| --- | --- |
| **Purpose** | Access external APIs, ML models, SaaS services, or code in any language from SQL |
| **Architecture** | Snowflake -> API Integration -> API Gateway -> Remote Service |
| **Setup Steps** | 1. Create API Integration with allowed endpoints 2. Create external function pointing to proxy URL |
| **Data Format** | JSON. Snowflake sends batches of rows. Remote service must return same # of rows. |
| **Latency** | Higher than native UDF. Best for batch calls, not row-by-row real-time |
| **Security** | Uses API Integration + IAM. Supports AWS PrivateLink, Azure Private Link |
| **Example** | ```sql
CREATE OR REPLACE EXTERNAL FUNCTION sentiment(text STRING)
RETURNS STRING
API_INTEGRATION = aws_api_gateway_integration
AS 'https://abc123.execute-api.us-east-1.amazonaws.com/prod/sentiment';

SELECT review, sentiment(review) FROM product_reviews;


**Key gotcha**: Remote service must handle batches, timeout < 30s, and follow Snowflake’s request/response spec.

#### **4. Sequence**
Database object that generates unique, incremental numeric values. Often used for surrogate keys.

| **Attribute** | **Details** |
| --- | --- |
| **Purpose** | Auto-incrementing IDs, order numbers. Alternative to `AUTOINCREMENT`/`IDENTITY` |
| **Properties** | `START`, `INCREMENT`, `MINVALUE`, `MAXVALUE`, `CYCLE` or `NO CYCLE` |
| **Scope** | Database schema object. Can be shared across multiple tables. |
| **Guarantees** | Unique if `NO CYCLE`. Not guaranteed gap-free. Not guaranteed chronological order. |
| **Performance** | Lightweight, cached. Use `ORDER = 1` for strict ordering but slower. |
| **Usage** | `sequence_name.NEXTVAL` to get next value. `CURRVAL` not supported. |
| **Example** | ```sql
CREATE OR REPLACE SEQUENCE order_seq 
  START = 1 
  INCREMENT = 1 
  COMMENT = 'Order ID generator';

CREATE OR REPLACE TABLE orders (
  order_id NUMBER DEFAULT order_seq.NEXTVAL,
  order_date TIMESTAMP,
  amount NUMBER
);

INSERT INTO orders(order_date, amount) VALUES (CURRENT_TIMESTAMP(), 99.95);
SELECT * FROM orders; -- order_id = 1


**Sequence vs IDENTITY**: `IDENTITY(1,1)` is table-bound. `SEQUENCE` is standalone and reusable across tables.


### **Quick Comparison**

| **Feature** | **UDF** | **Stored Procedure** | **External Function** | **Sequence** |
| --- | --- | --- | --- | --- |
| **Invoked by** | `SELECT my_udf(col)` | `CALL my_proc()` | `SELECT ext_func(col)` | `seq.NEXTVAL` |
| **Returns** | Scalar or table per row | One scalar value | Scalar per row | Number |
| **Can run DML/DDL** | No | Yes | No, but remote service can | No |
| **Runs where** | Snowflake warehouse | Snowflake warehouse | Outside Snowflake | Snowflake metadata |
| **Use case** | Transform data in query | Orchestrate multi-step job | Call ML model/API | Generate IDs |

Want me to add examples for Python UDFs or Snowpark procedures too?

</details>

<details> <summary>Snowflake Tasks & Streams</summary>
  
#### **Tasks**
- **What**: Object that schedules execution of SQL, stored procedures, or Snowflake scripting logic.
- **Use Cases**: Periodic data copies, maintenance routines, populating reporting tables. Any statement that needs to run on a schedule.
- **Requirements**: `ACCOUNTADMIN` role or custom role with `CREATE TASK` + global `EXECUTE TASK` privilege.
- **Key Components**:
    1. **Name**: Unique identifier per schema
    2. **Warehouse**: User-managed or Snowflake-managed serverless. Serverless can’t call Python/Java UDFs
    3. **Schedule**: Interval in minutes, CRON, OR trigger `AFTER` another task
    4. **SQL**: The statement to run
- **Start/Stop**: Tasks are created suspended. Use `ALTER TASK ... RESUME` to start, `SUSPEND` to pause. Needs `EXECUTE TASK` + `OWNERSHIP`/`OPERATE`.
- **DAGs**: Tasks can be chained into a Directed Acyclic Graph.
    - Root task has a schedule. Child tasks use `AFTER task1, task2` and run only when parents succeed
    - **Limits**: Max 1000 tasks per DAG, 100 dependencies per task, 100 children per task
    - All tasks in a DAG must have same owner, database, and schema

#### **Streams**
- **What**: Schema-level object that tracks `INSERT`, `UPDATE`, `DELETE` on a table between two points in time.
- **How**: `CREATE STREAM` on a table. Querying it returns only changed rows + 3 metadata columns:
    1. `METADATA$ACTION`: `INSERT` or `DELETE`
    2. `METADATA$ISUPDATE`: `TRUE` if part of an UPDATE. Updates show as `DELETE` + `INSERT` pair
    3. `METADATA$ROW_ID`: Unique row identifier to track changes over time
- **Offset**: Stream stores an offset marking where change tracking starts. Only changes after the offset appear.
- **Consume Changes**: Offset advances when the stream is used in a DML statement, e.g. `INSERT INTO target SELECT * FROM stream`.

#### **Tasks + Streams Together**
- **Pattern**: Use `SYSTEM$STREAM_HAS_DATA()` in a task to run only when stream has changes.
- **Flow**: Task scheduled -> checks stream -> if data, `INSERT` from stream to downstream table -> offset moves forward.
- **Result**: Continuous, incremental processing pipeline for CDC/ETL using deltas, not full table scans.

**Bottom Line**: Tasks = scheduling engine. Streams = change data capture. Combined = efficient, incremental data pipelines.
</details>

<details> <summary>Dynamic Tables</summary>

### 📌 Overview
- A **dynamic table** is a table that is **automatically refreshed** based on a defined query
- Combines concepts of:
  - Materialized views
  - Streams
  - Tasks
- Eliminates need for manual pipeline orchestration

---

### 🧠 Key Concept: Declarative Approach
- Define **what result you want (SQL query)**
- Snowflake handles:
  - Refresh logic
  - Scheduling
  - Dependency management

---

### 🔄 Core Functionality
- Continuously updates data based on **source tables**
- Maintains **data freshness automatically**
- No need for:
  - Streams
  - Tasks
  - Manual scheduling

---

### 🔗 Pipeline Capability
- Dynamic tables can be **chained together**
- Form **multi-step data pipelines**
- Snowflake automatically:
  - Detects dependencies
  - Executes in correct order
  - Ensures consistency

---

### ⏱️ Key Parameters
- **Target Lag**
  - Defines acceptable data freshness
  - Trade-off: freshness vs cost

- **Warehouse**
  - Compute used for refresh operations

- **Refresh Mode**
  - Incremental (efficient)
  - Full (recompute entire table)
  - Auto (Snowflake decides)

- **Initialize**
  - Controls initial population timing

---

### ⚙️ Behavior
- Incremental updates preferred for efficiency
- Automatically adapts to source data changes
- Supports **manual refresh trigger**
- Can be **suspended/resumed**

---

### 📊 Monitoring
- Track refresh status and history
- View:
  - Execution state (scheduled, running, failed, etc.)
  - Refresh type (incremental/full)
- UI provides **pipeline visualization**

---

### 🔐 Requirements (High-Level)
- Privileges to:
  - Create dynamic tables
  - Access source data
  - Use compute warehouse

---

### 🔑 Key Takeaways
- Simplifies **data pipeline creation**
- Fully **managed orchestration**
- Declarative vs imperative approach
- Ideal for **multi-step transformations**
- Balances **freshness, cost, and performance**
</details>


<details> <summary>Snowflake Parameters</summary>

### ⚙️ What are Parameters?
- **Settings to control behavior** of Snowflake components
- Used to **tune performance, execution, and defaults**


## 🧩 Types of Parameters

### 1. 🏢 Account Parameters
- Apply to the **entire account**
- Affect **all users & sessions**
- Set using: ALTER ACCOUNT

PERIODIC_DATA_REKEYING → controls encryption key rotation

2. 🔌 Session Parameters
Control behavior during an active session
A session = login → disconnect/timeout
📍 Can be set at 3 levels:
Level	Scope
Session	Current session only
User	Default for that user’s sessions
Account	Default for all sessions

- ALTER SESSION
- ALTER USER
- ALTER ACCOUNT

Precedence (highest → lowest):
Session > User > Account

Example: DATE_OUTPUT_FORMAT

3. 📦 Object Parameters
Control runtime behavior of objects
Apply to:
Warehouses, Databases, Tables, etc.

Set using:

ALTER <object>
⚖️ Parameters vs Properties
Aspect	Parameters	Properties
Purpose	Runtime behavior	Structure/configuration
Examples	Timeout, concurrency	Warehouse size, auto_suspend
Nature	Dynamic	Static

🔄 Object Parameter Scope & Precedence
Can be set at:
Account level → default for all objects
Object level → overrides default
Example Hierarchy:
Account (default)
   ↓
Database (default for tables)
   ↓
Table (overrides database)
🔑 Key Takeaways
Account params → global settings
Session params → execution behavior (multi-level + precedence)
Object params → runtime control of objects
Lower-level settings override higher-level defaults
Understand scope + precedence → critical for exam

</details>
