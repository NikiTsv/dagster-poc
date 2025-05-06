1. create and activate venv
2. pip install dagster dagster-webserver pandas dagster-duckdb

# Dagster Components Overview

## 🧱 Core Components

### **Asset**
A first-class, declarative representation of data. Assets describe what your pipeline produces or consumes (e.g., a table, file, or ML model), replacing traditional DAG-centric tasks.

### **Op**
The smallest unit of computation. An `@op` is a function that performs a step in your pipeline and returns an output.

### **Graph**
A composition of ops. It defines the flow of data and dependencies between them.

### **Job**
An executable version of a graph. A job includes configuration, resources, execution policies, etc.

### **Schedule**
Defines time-based triggers to run jobs (e.g., daily at midnight).

### **Sensor**
Event-based trigger. Watches for changes (e.g., file added, new record in DB) and kicks off jobs accordingly.

### **IO Manager**
Handles how inputs/outputs of ops/assets are stored and retrieved (e.g., local file system, S3, DuckDB, etc.).

### **Partition**
Represents data slices over time or dimensions (e.g., daily partitioned data). Helps with reprocessing only what's needed.

### **Resource**
A managed external system or dependency (e.g., a database client, Spark session, HTTP service). Injected into ops/assets via config.

### **Type**
Defines custom types and validation logic for inputs/outputs. Helps enforce data contracts.

### **Config**
Describes runtime configuration for jobs, ops, and resources. Often passed in via YAML or Python dict.

### **AssetCheck**
Used to validate or assert properties of assets, such as data quality checks (e.g., null rate, row count, schema match).


---
## 🏗️ Dagster Infrastructure Components (Simplified Overview)

| Component              | Role                                             | Quantity/Deployment Notes        |
|------------------------|--------------------------------------------------|----------------------------------|
| **Dagster Webserver**  | Serves the Dagster UI (`Dagit`) and GraphQL API | 1 or more                        |
| **Dagster Daemon**     | Handles schedules, sensors, and run queueing     | 1                                |
| **Code Location Server** | Loads and serves Dagster definitions (repos, jobs, assets) | 1 per code location              |
| **Persistent Storage** | Stores run, event, and schedule metadata         | 1 (external DB like PostgreSQL)  |
| **Executors/Launchers**| Executes runs (e.g., Local, Docker, K8s)         | Configurable per deployment mode |
