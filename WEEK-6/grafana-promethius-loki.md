# How to Import a Dashboard in Grafana

## Steps to Import a Dashboard

1. Click on `+` and select `Import Dashboard`.
2. Paste the JSON code for the dashboard.
3. Upload the code and click on `Load`.
4. The dashboard will display the configured panels.

## Learning Prometheus and Loki Queries for Grafana
# Prometheus and Loki Basics for Grafana

## Prometheus Basics

- **Purpose**: A time-series database for monitoring and alerting.
- **Data Collection**: Pulls metrics via HTTP endpoints.
- **Storage**: Stores time-series data efficiently using a custom TSDB (Time Series Database).
- **Querying**: Uses PromQL to retrieve and analyze data.
- **Use Case**: Best for tracking application and infrastructure metrics (e.g., CPU, memory, request count).

## Loki Basics

- **Purpose**: A log aggregation system designed to work with Grafana.
- **Data Collection**: Ingests logs from applications but does not index the log content (only labels).
- **Querying**: Uses LogQL to filter, parse, and analyze logs.
- **Use Case**: Best for troubleshooting and correlating logs with metrics.

## Differences Between Prometheus and Loki

| Feature           | Prometheus                               | Loki                                      |
|-------------------|------------------------------------------|-------------------------------------------|
| **Data Type**     | Metrics (time-series)                    | Logs                                      |
| **Query Language**| PromQL                                   | LogQL                                     |
| **Storage**       | Custom TSDB                              | Label-based log storage                  |
| **Indexing**      | Indexes all time-series data             | Only indexes labels, not log content     |
| **Use Case**      | Application & infra monitoring           | Log aggregation & debugging              |

### Prometheus Basics

Prometheus is a time-series database used for monitoring and alerting. PromQL (Prometheus Query Language) is used to query data.

#### Basic PromQL Concepts:

1. **Metrics and Labels**: Metrics are time-series data points with labels (key-value pairs).
   - Example: `http_requests_total{status="200", method="GET"}`

2. **Key Functions**:
   - `rate()`: Calculate per-second rate of increase.
   - `sum()`: Aggregate values.
   - `by()`: Group by specific labels.
   - `avg()`, `min()`, `max()`: Statistical operations.

### Loki Basics

Loki is a log aggregation system designed to work with Grafana. LogQL is used to query logs.

#### Basic LogQL Concepts:

1. **Log Stream Selection**: Uses labels to select log streams.
   - Example: `{app="nginx", env="production"}`
2. **Log Content Filtering**:
   - Example: `{app="nginx"} |= "ERROR"`
3. **Log Parsing and Metrics Extraction**:
   - Use `| pattern` or `| json` operators to extract fields.

## Using Prometheus in Grafana

### Step 1: Add Prometheus Data Source

1. Navigate to `Configuration > Data Sources`.
2. Click `Add data source`.
3. Select `Prometheus`.
4. Enter Prometheus server URL (e.g., `http://localhost:9090`).
5. Click `Save & Test`.

### Step 2: Create a Dashboard

1. Click `+ Create > Dashboard`.
2. Click `Add new panel`.

### Step 3: Write Prometheus Queries

1. Select `Prometheus` as the data source.
2. Enter the PromQL query:
   ```promql
   rate(http_requests_total{job="api-server"}[5m])
   ```
3. Use the "Metrics browser" to explore available metrics.
4. Apply functions using the Function dropdown.

## Using Loki in Grafana

### Step 1: Add Loki Data Source

1. Navigate to `Configuration > Data Sources`.
2. Click `Add data source`.
3. Select `Loki`.
4. Enter Loki server URL.
5. Click `Save & Test`.

### Step 2: Create a Logs Panel

1. Add a new panel to the dashboard.
2. Select `Logs` as the visualization.

### Step 3: Write Loki Queries

1. Select `Loki` as the data source.
2. Enter a LogQL query:
   ```logql
   {app="nginx"} |= "ERROR" | json
   ```
3. Use the "Log browser" to explore available log streams.

## Common Prometheus Query Examples

1. **CPU Usage**:
   ```promql
   100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
   ```
2. **Memory Usage**:
   ```promql
   (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100
   ```
3. **HTTP Error Rate**:
   ```promql
   sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) * 100
   ```
4. **Request Latency (99th percentile)**:
   ```promql
   histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))
   ```

## Common Loki Query Examples

1. **Filter for Error Logs**:
   ```logql
   {app="myapp"} |= "ERROR"
   ```
2. **Count Error Logs by Service**:
   ```logql
   sum by(service) (count_over_time({app="myapp"} |= "ERROR" [5m]))
   ```
3. **Extract and Count Status Codes**:
   ```logql
   {app="nginx"} | pattern <_> - - <_> "<_> <_> <_>" <status> <_> | count_over_time([5m]) by (status)
   ```
4. **Parse JSON Logs**:
   ```logql
   {app="api"} | json | response_time > 200
   ```

Practiced with these examples in my Grafana instance to get comfortable with both query languages. Started with simple queries and gradually increase complexity as I got more familiar with the syntax.
![Screenshot (53)](https://github.com/user-attachments/assets/2caf0f60-b78a-439d-87ee-269e1a16a827)
## Steps to Import a Dashboard

   1.Click on + and select Import Dashboard.
   
   2.Paste the JSON code for the dashboard.
   
   3.Upload the code and click on Load.
   
   4.The dashboard will display the configured panels.


![Screenshot (54)](https://github.com/user-attachments/assets/4c1343e2-6784-433d-aa86-5b31578ff5e7)

## Steps to Export a Dashboard
   
   1.Open the dashboard you want to export.
   
   2.Click on the Share icon (usually located in the top-right corner).
   
   3.Select the Export tab.
   
   4.Click Save JSON to download the dashboard configuration as a JSON file.

You can now share or import this JSON file into another Grafana instance.
