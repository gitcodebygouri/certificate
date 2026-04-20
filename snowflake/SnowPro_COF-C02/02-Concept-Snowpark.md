<details><summary>Snowpark</summary>

**Snowpark** is a developer framework in **Snowflake** that enables writing data transformation and processing logic using familiar programming languages instead of SQL.

### Key Points
- Supports **Python, Java, and Scala**
- Allows writing **DataFrame-style code** (similar to Spark)
- Executes code **inside Snowflake** (no data movement)
- Enables **complex transformations, UDFs, and stored procedures**
- Integrates with Snowflake’s **compute engine for scalability**

### Benefits
- Reduces need for complex SQL
- Improves developer productivity
- Keeps data secure within Snowflake
- Leverages Snowflake’s performance and scaling

### Use Cases
- Data engineering pipelines  
- Machine learning preprocessing  
- Complex business logic transformations  
- Building data applications within Snowflake

</details>

<details><summary>Snowflake AI Data Cloud & Cortex</summary>

### Overview
- Snowflake rebranded to **AI Data Cloud (March 2025)** to emphasize AI capabilities.
- Focus: integrating **LLMs (Large Language Models)** and ML directly into the platform.

---

## AI Feature Categories

### 1. Cortex (LLM-Based Features)
- Uses **large language models** for generative AI tasks.
- Supports models from providers like OpenAI, Anthropic, Meta, Mistral, etc.
- Handles:
  - Text generation
  - Summarization
  - Q&A
  - Translation
  - Conversational use cases

#### Key Components

**Cortex SQL**
- Extends SQL with AI functions (e.g., `AI_COMPLETE`)
- Inputs:
  - Prompt (text input)
  - Model name (LLM)
- Optional controls:
  - Temperature → randomness
  - Guardrails → safety filtering
- Use case: summarizing text, querying data in natural language

**Snowflake Copilot**
- AI-powered SQL assistant in UI
- Converts **natural language → SQL queries**
- Understands schema (tables, columns, metadata)
- Can:
  - Generate queries
  - Optimize SQL
  - Explain features
- Limitation: weaker if metadata/comments are missing

**Document AI**
- Extracts structured data from **unstructured documents (PDFs)**
- Process:
  1. Create model build
  2. Provide sample documents
  3. Define extraction questions
  4. Validate & optionally fine-tune
  5. Publish model
- Output: JSON with extracted values + confidence scores
- Uses Snowflake LLM (e.g., Arctic-Tilt)

---

### 2. Machine Learning (Traditional ML)
- Built-in ML functions for:
  - Classification
  - Forecasting
- Supports creation of custom ML models/functions

---

## Key Benefits
- AI runs **inside Snowflake** (no data movement)
- Combines **data + AI + SQL in one platform**
- Simplifies building **AI-powered data pipelines and apps**
- Supports both **generative AI (Cortex)** and **traditional ML**

---
</details>
<details><summary>Snowflake Access Control</summary>

### Overview
- Access control defines **who can do what on which objects**.
- Core to Snowflake security model.
- Based on **roles, privileges, and object hierarchy**.

---

## Access Control Frameworks

### 1. Role-Based Access Control (RBAC) — Primary
- Privileges are assigned to **roles**, not directly to users.
- Roles are then assigned to users.
- Supports **principle of least privilege**.
- Roles align with business functions (e.g., Admin, Analyst).

---

### 2. Discretionary Access Control (DAC)
- Every object has an **owner role**.
- Owner has full control:
  - Read, modify, delete
  - Grant/revoke access
  - Transfer ownership
- Works alongside RBAC.

#### Managed Access Schema
- Special schema type.
- Only **schema owner** can grant privileges (not object owners).

---

### 3. User-Based Access Control (UBAC)
- Privileges assigned **directly to users**.
- Useful for **temporary or exceptional access**.
- Not recommended as primary approach (hard to manage at scale).

---

## Securable Objects
- Most Snowflake objects are **securable** (can have permissions).
- Each object:
  - Has a **single owner role**
  - Supports **granular privileges** (e.g., SELECT, MODIFY)

---

## Privileges Example
- On warehouses:
  - `USAGE` → run queries
  - `MODIFY` → change configuration

---

## Object Hierarchy & Access
- Access often requires permissions on **parent objects**:
  - Account → Database → Schema → Table
- Example:
  - To create a table → need access to **database + schema**

---

## Key Rules
- Owner gets **all privileges by default**
- Ownership can be **transferred**
- Roles with **MANAGE GRANTS** can:
  - Grant/revoke access on any object
- Access is **denied by default** unless explicitly granted


## Key Takeaways
- **RBAC is the foundation** of Snowflake security
- DAC adds **ownership-based control**
- UBAC is for **edge cases only**
- Security is **granular, hierarchical, and explicit**

<img width="1431" height="908" alt="image" src="https://github.com/user-attachments/assets/a7ee64c7-99e8-48b6-bd83-8cc2bba42538" />
<img width="1491" height="854" alt="image" src="https://github.com/user-attachments/assets/a32971bf-e4c8-4292-a31b-f062a78f0f40" />

</details>

<details>
  
<summary> Snowflake Security Privileges </summary>

- A **privilege** defines what actions a role can perform on a securable object.
- Typically structured as: **GRANT verb  ON  verb **
- Enables **fine-grained access control**.

## Privilege Categories

### 1. Global Privileges
- Apply at **account level**
- Examples:
  - Create databases
  - Monitor usage
  - MANAGE GRANTS (control access across objects)


### 2. Account Object Privileges
- Apply to **account-level objects** (e.g., databases)
- Example:
  - MODIFY  change database settings


### 3. Schema Privileges
- Control actions within a **schema**
- Example:
  - USAGE  access schema

### 4. Schema Object Privileges
- Apply to objects inside schemas (e.g., tables, views)
- Examples:
  - SELECT  read data
  - TRUNCATE  remove data

## Key Concepts
- Privileges can be **combined** for granular control
- Some privileges are **common across objects**, others are **object-specific**
- Access is enforced via **roles**

## Managing Privileges

### GRANT
- Assign privileges to roles

### REVOKE
- Remove privileges from roles

- Only allowed by:
  - **Object owner**, or
  - Role with **MANAGE GRANTS**


## Future Grants
- Define privileges for **objects not yet created**
- Example:
  - Automatically grant `SELECT` on all future tables in a schema
- Simplifies ongoing access management

### Limitations
- Not supported for:
  - Data sharing
  - Replication
  - Masking policies
  - Row access policies


## Key Takeaways
- Privileges control **actions on objects**
- Organized into **4 categories**
- Managed via **GRANT / REVOKE**
- **Future grants** automate access for new objects
<img width="872" height="778" alt="image" src="https://github.com/user-attachments/assets/d6a3bc2f-aece-47c7-8bec-f34a3e2aadb1" />

</details>

<details><summary>Snowflake Authentication & Identity</summary>

### Key Pair Authentication
- Uses **public-private key pair** instead of username/password
- Primarily for **Snowflake clients/connectors (not UI)**
- Setup:
  1. Generate key pair (min **2048-bit**) using OpenSSL
  2. Assign **public key** to Snowflake user
  3. Keep **private key** securely on client side
- Used in connectors (e.g., Python) for authentication
- Supports **key rotation**:
  - Two active keys allowed
  - Enables **zero-downtime key updates**

---

### OAuth (Authorization)
- Based on **OAuth 2.0 protocol**
- Enables **secure access without sharing credentials**
- Supports **delegated authorization** (client acts on user’s behalf)
- Use case: tools like BI apps connecting to Snowflake
- Types:
  - Snowflake OAuth
  - External OAuth

---

### SCIM (User & Role Management)
- Stands for **System for Cross-domain Identity Management**
- Used for **automated user and role lifecycle management**
- Integrates with **Identity Providers (IdP)** (e.g., ADFS)
- Capabilities:
  - Create, update, delete users
  - Manage roles
  - Map external groups → Snowflake roles
- Uses **REST API (SCIM API)**

---

## Key Takeaways
- Key Pair Auth = **secure client authentication**
- OAuth = **credential-free delegated access**
- SCIM = **automated identity & access management**
---
</details>

<details><summary>Data Encryption</summary>
  
<img width="1482" height="865" alt="image" src="https://github.com/user-attachments/assets/900ef886-bfa0-4a21-bdbb-7883ed2f10d6" />

### Overview
- Snowflake provides **automatic encryption** for all data.
- No user configuration required (**fully managed & transparent**).

---

## Encryption Types

### 1. Encryption at Rest
- All stored data is encrypted using **AES-256**
- Applies to:
  - Tables (storage layer)
  - Internal stages (files)
  - Query result cache
  - Virtual warehouses
- Ensures all Snowflake-managed data is **secure at rest**

---

### 2. Encryption in Transit
- All network communication secured via:
  - **HTTPS + TLS 1.2**
- Covers:
  - Queries
  - Data loading/unloading
- Ensures **secure data transfer**

---

### 3. End-to-End Encryption
- Combines:
  - Encryption at rest + in transit
- Protects data **throughout entire lifecycle**

---

## Data Loading & Stages

### Internal Stage
- Files automatically encrypted during upload
- Encryption occurs:
  - On client (upload)
  - Again in storage (separate key)

### External Stage
- User-managed (e.g., S3)
- Options:
  - Unencrypted → encrypted when loaded into Snowflake
  - Pre-encrypted → decrypted then re-encrypted in Snowflake

---

## Key Management (Hierarchical Model)
- Uses **multi-level key hierarchy**:
  - Account Master Key
  - Table Master Key
  - File/Micro-partition Key
- Each level **encrypts the level below (key wrapping)**

### Benefits
- Limits impact of key compromise
- Provides **strong isolation (account/table/file level)**

---

## Key Rotation & Re-Keying

### Key Rotation
- Automatic rotation every **30 days**
- Old keys retained for decryption only

### Periodic Re-Keying (Enterprise Feature)
- Re-encrypts old data with new keys after ~1 year
- Improves security but adds **cost**

---

## Root Key Protection
- Stored in **cloud hardware security modules (HSM)**
- Example:
  - AWS CloudHSM
- Ensures secure key generation and storage

---

## Tri-Secret Secure
- Uses **composite key model**:
  - Snowflake-managed key + Customer-managed key
- Customer key stored in cloud KMS:
  - AWS KMS / Azure Key Vault / GCP KMS

### Benefits
- Full control over data access
- Can revoke access by disabling customer key

### Trade-offs
- Increased **management overhead**
- Requires Snowflake support to enable

</details>

<details><summary>Snowflake Dynamic Data Masking</summary>
  
### Overview
- **Dynamic Data Masking** = column-level security applied **at query runtime**
- Data is **not stored masked** (unlike static masking)
- Controls **who sees data and how it is masked**


## How It Works
- A **masking policy** is applied to a column (table/view)
- At query time:
  - **Authorized users** → see full data
  - **Unauthorized users** → see masked/partial data
- Fully **transparent to users**

---

## Masking Policy Basics
- Defined using `CREATE MASKING POLICY`
- Components:
  - Input value (column reference)
  - Input/output data type (must match)
  - Policy logic (usually `CASE` conditions)
- Example logic:
  - Show full value for certain roles
  - Mask or partially mask for others

---

## Masking Techniques
- Full masking (e.g., `****`)
- Partial masking (e.g., show email domain only)
- Hashing (e.g., SHA-2)
- Custom logic via UDFs

---

## Key Features
- Applied using `ALTER TABLE` (or during creation)
- Can be applied to **multiple columns across schemas**
- **Schema-level object**
- Supports **segregation of duties** (security teams manage policies)

---

## Advanced Behavior
- **Policy nesting supported**
  - Lower-level policy applied first
- Applies **everywhere column is used**
  - Includes joins, filters, etc.
- Joins on masked columns use **masked values**, not raw data

---

## External Tokenization
- Stores **tokenized (scrambled) data** instead of raw values
- Masking policy calls **external function** to detokenize at runtime
- Flow:
  1. Store tokenized data
  2. Query triggers masking policy
  3. External service returns real value (if authorized)

### Benefits
- Even Snowflake/cloud provider **cannot read raw data**
- Stronger security for sensitive data

---

## Key Takeaways
- Dynamic masking = **runtime data protection**
- Policies control **visibility based on roles**
- Supports **flexible masking strategies**
- External tokenization adds **extra security layer**
---
</details>
<details><summary> Snowflake Secure Views</summary>

### Overview
- **Secure Views** protect access to:
  - Underlying tables
  - View definition and internal logic
- Can be applied to:
  - Standard views
  - Materialized views

---

## Creation & Management
- Created using `CREATE SECURE VIEW`
- Can be reverted using `ALTER VIEW UNSET SECURE`
- Owned by a role which controls access

---

## Key Security Features

### 1. Metadata & Definition Protection
- Underlying table details are **hidden**
- View definition visible only to **authorized users**
- Not exposed in:
  - `SHOW VIEWS`
  - `GET_DDL`
  - Information Schema
  - Account Usage views

---

### 2. Query Optimization Restrictions
- Disables certain **query optimizer behaviors** (e.g., pushdown)
- Prevents indirect data exposure via query execution plans
- Ensures filtered data **cannot be inferred**

---

## Trade-Offs
- **More secure** than standard views
- May be **slower** due to limited optimizations
- Should be used only when **strict data security is required**

---

## Key Takeaways
- Secure views protect both **data and metadata**
- Prevent unintended data exposure via optimization
- Trade performance for **enhanced security**
---
</details>
<details><summary>Snowflake ACCOUNT_USAGE & INFORMATION_SCHEMA</summary>
<img width="1660" height="874" alt="image" src="https://github.com/user-attachments/assets/bea8fa47-5ad5-4811-bc5c-eea02611aaf7" />
<img width="1681" height="877" alt="image" src="https://github.com/user-attachments/assets/b5ad52b5-1515-4c7e-8bfd-4dcf73355b51" />

### SNOWFLAKE Database Overview
- Default **read-only shared database** in every account
- Provided via **ACCOUNT_USAGE share**
- Contains **historical usage and metadata views**

---

## Key Schemas in SNOWFLAKE DB

### 1. ACCOUNT_USAGE
- Main schema for:
  - Object metadata
  - Usage metrics (queries, credits, etc.)
- Example: table metadata, query history

### 2. CORE
- Contains **system tags** (data classification)
- Limited but expanding

### 3. READER_ACCOUNT_USAGE
- Metrics for **reader accounts**

### 4. DATA_SHARING_USAGE
- Info on **data sharing/listings**

### 5. ORGANIZATION_USAGE
- Usage across **all accounts in org**

### 6. INFORMATION_SCHEMA
- Standard schema (exists in every database)
- Covered separately below

---

## ACCOUNT_USAGE Key Features
- Tracks **historical data (up to 1 year)**
- Includes **dropped objects** (`DELETED` column)
- Has **latency (~2 hours)**
- Useful for:
  - Billing analysis
  - Query monitoring
  - Warehouse usage tracking

---

## INFORMATION_SCHEMA Overview
- Available in **every database**
- Based on **ANSI SQL standard**
- Contains:
  - Views (metadata)
  - Table functions (usage/history)

---

## INFORMATION_SCHEMA Key Features
- **Real-time / no latency**
- Shows only **accessible objects (role-based)**
- Does **not include dropped objects**
- Retention:
  - ~7 days to 6 months (varies)

---

## Views vs Table Functions
- **Views** → metadata (tables, schemas)
- **Table functions** → historical data (e.g., `COPY_HISTORY`)

---

## ACCOUNT_USAGE vs INFORMATION_SCHEMA

| Feature              | ACCOUNT_USAGE        | INFORMATION_SCHEMA     |
|---------------------|---------------------|------------------------|
| Data Freshness      | Delayed (~2 hrs)    | Real-time              |
| Historical Data     | Up to 1 year        | Limited (7d–6m)        |
| Dropped Objects     | Included            | Not included           |
| Scope               | Account-wide        | DB + account-level mix |
| Access              | Requires privileges | Role-based filtering   |

---

## Access Control
- Default access: **ACCOUNTADMIN role only**
- Can grant access to other roles

---

## Key Takeaways
- Use **ACCOUNT_USAGE** for historical analysis
- Use **INFORMATION_SCHEMA** for real-time insights
- Both provide **metadata + usage tracking** with different trade-offs
---

</details>
<details><summary>Snowflake Alerts & Notifications</summary>

### Snowflake Alerts
- **Schema-level object** that:
  - Periodically evaluates a **SQL condition**
  - Executes an **action** when condition = TRUE
- Acts like a **scheduled watchdog**

---

## Core Components

### 1. Condition
- SQL query (often using `EXISTS`)
- Returns TRUE if query produces rows

### 2. Action
- SQL statement executed when condition is met
- Examples:
  - Insert into table
  - Call stored procedure

### 3. Schedule / Trigger
- Defines how often condition is evaluated:
  - Fixed interval (e.g., every minute)
  - Cron expression

---

## Trigger Types

### Scheduled Alerts
- Run at defined intervals

### Event-Based Alerts
- Triggered on **new data**
- Requires:
  - `CHANGE_TRACKING = TRUE`
- Limitations:
  - Single table/view only
  - No joins, CTEs, DML, or stored procedures in condition

---

## Compute Options

### Warehouse-Based
- Uses specified warehouse
- Billing:
  - Per second (min 60 sec per run)

### Serverless (No warehouse specified)
- Snowflake auto-manages compute
- Billing:
  - Based on actual usage
- Recommended for **infrequent triggers**

---

## Alert Lifecycle
- Created in **SUSPENDED state**
- Key operations:
  - `RESUME` → activate
  - `SUSPEND` → pause
  - `EXECUTE ALERT` → manual run (testing)
  - Modify:
    - Condition
    - Action
    - Schedule

---

## Permissions
- Requires **EXECUTE ALERT** privilege
- Default: ACCOUNTADMIN role

---

# Notifications

### Overview
- Alerts trigger actions, **notifications deliver messages**
- Sent via **Notification Integrations** (account-level object)

---

## Delivery Methods
- **Email**
- **Cloud queues**:
  - AWS SNS
  - Azure Event Grid
  - GCP Pub/Sub
- **Webhooks** (HTTP endpoints, e.g., Slack, Teams)

---

## Notification Integration
- Stores connection details to external systems
- Types:
  - `EMAIL`
  - `QUEUE`
  - `WEBHOOK`
- Requires **CREATE INTEGRATION** privilege

---

## Email Notifications Setup
1. Verify user email
2. Create notification integration (type = EMAIL)
3. Use `SYSTEM$SEND_SNOWFLAKE_NOTIFICATION` in alert action

### Notes
- Can only send to **users within same Snowflake account**

---

## How Alerts + Notifications Work Together
- Alert condition → TRUE  
- Action → calls notification procedure  
- Notification → message delivered (email/queue/webhook)

---

## Key Takeaways
- Alerts = **decision layer**
- Notifications = **delivery layer**
- Supports **automation & monitoring workflows**
- Serverless alerts optimize **cost for infrequent events**
---

</details>
<details><summary></summary></details>

<details><summary></summary></details>
<details><summary></summary></details>
