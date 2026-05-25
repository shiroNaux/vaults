---
tags:
  - DataArchitecture
  - Delta
  - spark
---
# Glossaries
- Z-odering
# Abstraction

Delta lake là 1 open table format
Delta lake không phải là file format như [[Parquet|parquet]], mà thực tế là Delta lake sử dụng parquet để lưu trữ data. 


# Features

## Liquid clustering
## Deletion vectors

# MERGE
The core **MERGE (upsert)** mechanism in Delta Lake is consistent between the open-source version and the Databricks-managed version because they share the same underlying **Delta Lake storage protocol** and **transaction log** (`_delta_log`).

### How the MERGE Operation Works

Regardless of the environment, the `MERGE` command follows these fundamental steps to ensure ACID compliance:

1. **Transaction Log Analysis:** Delta reads the `_delta_log` to identify the current state of the table and the relevant files.
2. **File Selection:** It identifies which specific Parquet files contain rows that match the join conditions of your merge query.
3. **Join Execution:** The system performs a join between the source data and the target Delta table to categorize rows into "matches" (for updates) and "non-matches" (for inserts).
4. **File Rewrite (Copy-on-Write):** This is the engine of the operation. Affected Parquet files are rewritten to include the changes. New files are created, and old files are marked for removal in the log.
5. **Atomic Commit:** The transaction log is updated to atomically "remove" the old files and "add" the new ones, ensuring that the version increment is visible to readers as a single, consistent state.

### Where They Differ: Performance and Implementation

While the _logic_ is the same, the _efficiency_ of how these steps are executed differs significantly:

- **Databricks-Specific Optimizations:** Databricks uses proprietary enhancements like **Deletion Vectors** (which allow for marking rows as deleted without needing to rewrite entire Parquet files immediately) and the **Photon engine** (a high-performance vectorized query engine) to significantly speed up the "Join Execution" and "File Rewrite" phases.
- **Maintenance Automation:** Databricks can automatically handle file compaction and vacuuming using background processes. In open-source Delta Lake, you often have to manually trigger `OPTIMIZE` and `VACUUM` commands to clean up the "removed" files left behind after merges.
- **Conflict Handling:** Databricks provides advanced **Optimistic Concurrency Control (OCC)** settings and better management of write conflicts in high-concurrency environments, whereas open-source users must typically rely on standard configurations.