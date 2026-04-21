# 🧊 Snowflake Virtual Warehouse – Performance

## 🔹 Core Concept
**Virtual Warehouse = Compute Layer in Snowflake**
- Executes: queries, DML, data loading
- Built on MPP (Massively Parallel Processing)
- **Separation of Compute & Storage** → independent scaling + ACID compliance

## 🔹 Architecture & Behavior
- Composed of multiple worker nodes (VMs)
- Fully managed abstraction: no infra control needed
- Supports parallel query execution
- Uses **local SSD cache** → improves repeated query performance

## 🔹 Key Properties
- **Fast provisioning**: near-instant start/stop
- **Resizable anytime**
- **Suspend/Resume** supported
- **Independent warehouses** → workload isolation

## 🔹 Warehouse States
- **Started**: running, consuming credits
- **Suspended**: no compute, no cost
- **Resizing**: can occur during execution

## 🔹 Automation
### Auto Suspend
- **Default**: 600 sec
- **Min effective**: ~60 sec due to billing constraint
- Too low → ❌ cost inefficiency + cache loss

### Auto Resume
- **Default**: TRUE
- Auto-starts warehouse on query

### Initially Suspended
- **Default**: FALSE
- Useful to avoid immediate billing

## 🔹 Cost & Billing
- Charged **only when Started**
- **Per-second billing**, min 60 sec
- Cost depends on:
    - Warehouse size
    - Active time
- ❗ **Not based on number of queries**

## 🔹 Sizing (T-Shirt Model)
**XS → 6XL**

Each step up:
- ~2× compute
- ~2× cost

**Best Practice**:
- Start small → scale up

**Workload Fit**:
- **Small queries**: no benefit from scaling
- **Complex queries**: benefit from larger size
- **Data loading**: depends more on parallelism than size

## 🔹 Multi-Cluster Warehouses (Scaling Out)
Used for **concurrency**, not performance of single query

**Key Params**:
- `MIN_CLUSTER_COUNT`
- `MAX_CLUSTER_COUNT`

**Modes**:
- **Maximized**: MIN = MAX, always running
- **Auto-scale**: dynamic scaling

**Billing**:
- Total cost = sum of all active clusters
- Each cluster billed independently

## 🔹 Scaling Policies
| Policy | Scale-out | Scale-in | Notes |
| --- | --- | --- | --- |
| **STANDARD** (Default) | Immediate | ~2 mins | Balanced cost + performance |
| **ECONOMY** | Delayed (~6 min) | Slower (~6 min) | Cost optimized, higher latency |

## 🔹 Concurrency Controls
- `MAX_CONCURRENCY_LEVEL` → parallel query limit
- `STATEMENT_QUEUED_TIMEOUT` → max queue wait
- `STATEMENT_TIMEOUT` → max execution time

## 🔹 Performance Features
- **Warehouse Cache (SSD)** → lost on suspend
- **Scaling Up** → improves query speed
- **Scaling Out** → improves concurrency

## 🔹 Query Acceleration Service (QAS)
**Serverless feature** that adds temporary compute for complex queries

**Works on**:
- Large scans
- Parallel workloads

**Key Points**:
- Fully managed by Snowflake
- Uses scale factor, default = 8
- Billed per second, only when used

**Difference**:
- **QAS** → speeds single query
- **Multi-cluster** → handles concurrency

## 🔹 Resource Monitors (Cost Control)
Set credit quotas + thresholds

**Actions**:
- `NOTIFY`
- `SUSPEND`
- `SUSPEND_IMMEDIATE`

**Works at**:
- Account level
- Warehouse level

❗ **Not real-time**: latency exists

## 🔹 Monitoring Usage
**UI**:  
Admin → Usage → Compute

**SQL**:  
`WAREHOUSE_METERING_HISTORY` (Account Usage)
- **Retention**: 1 year
- **Latency**: up to 180 min

## 🔹 Generation 1 vs Generation 2
| Feature | Gen1 | Gen2 |
| --- | --- | --- |
| **Performance** | Standard | Improved |
| **Efficiency** | Baseline | Better |
| **Query Speed** | Normal | Faster |

✅ **Exam Tip**: 
- Same size → **Gen2 performs better**
- Prefer **Gen2** for new workloads

## 🚀 High-Value Exam Takeaways
- **Virtual warehouse = compute layer**
- **Auto suspend/resume** → key for cost control
- **Per-second billing** with min 60s
- **Multi-cluster = concurrency scaling**
- **QAS = serverless query acceleration**
- **Cache lost on suspend**
- **Gen2 > Gen1**
- **Resource monitors ≠ real-time enforcement**

---

# Snowflake Performance & Optimization — Concise Summary

## Query Performance Fundamentals
- SQL quality directly impacts performance and cost.
- Poor queries increase execution time and warehouse usage.
- Monitor using:
    - **Query History** → quick insights (execution time, bytes scanned).
    - **Query Profile** → deep analysis (bottlenecks, CPU, I/O).
    - **ACCOUNT_USAGE / INFORMATION_SCHEMA** → historical & near real-time tracking.
- Optimize via partition pruning, join tuning, and warehouse sizing.

## Query Optimization Best Practices
- **Execution Order**: FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
- Apply filters early to reduce data scanned.
- **Joins**: avoid non-unique keys to prevent duplication.
- **ORDER BY**: expensive; use with LIMIT and avoid repetition.
- **GROUP BY**: low-cardinality columns perform better than high-cardinality ones.
- Balance data reduction vs warehouse size to avoid memory spilling.

## Caching Mechanisms
- **Metadata Cache** → no compute needed (fastest for simple queries).
- **Results Cache** → reuses identical query results (24 hrs+).
- **Local Disk Cache** → warehouse-level data caching (lost on suspend).

**Execution Priority**: Metadata → Results → Local → Remote (slowest & costliest)

## Materialized Views (MV)
- Precomputed, stored query results with automatic refresh.
- Improve performance for repeated complex queries.
- Best for read-heavy, low-change data.
- **Limitations**: single table, no joins/window functions.
- **Costs** include storage + serverless refresh compute.

## Clustering & Micro-Partitioning
- Improves data pruning by organizing data efficiently.
- **Key metrics**:
    - Overlap (↓ better)
    - Depth (↓ better)
    - Best case: no overlap → minimal partitions scanned.
- Degrades over time due to DML → auto re-clustering fixes it.

**Clustering Keys Guidelines:**
- Use frequently filtered/joined columns.
- Prefer moderate cardinality.
- Limit to 3–4 columns (low → high cardinality order).

## Clustering Performance & Monitoring
- Use `CLUSTERING_INFORMATION` to assess quality.
- Good clustering → fewer partitions scanned → faster queries.
- Monitor cost via `AUTOMATIC_CLUSTERING_HISTORY`.
- Most effective for large, frequently queried tables.

## Search Optimization Service (SOS)
- Enhances point lookup queries (returns few rows).
- Supports equality and `IN` filters on common data types.
- Works via a search access path for faster partition lookup.
- Best for large tables with selective queries (not analytics).
- **Trade-offs**:
    - Storage cost (~25% of table size)
    - Serverless compute cost (increases with DML)

## Final Takeaways
- Reduce scanned data → biggest performance gain.
- Use Query Profile to identify bottlenecks.
- Leverage caching, clustering, and SOS strategically.
- Balance performance improvements vs cost overhead.


