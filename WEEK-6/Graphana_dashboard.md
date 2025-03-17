
GRAFANA(Week-6,Day-1)

We were introduced to Grafana and Prometheus in SRE.

## Toil:
In Site Reliability Engineering (SRE), toil refers to the repetitive, manual,
and automatable work that is necessary to operate a system but does not
contribute to its long-term improvement. It’s the kind of work that takes time
away from more valuable engineering tasks like improving reliability,
scalability, and performance.

### SREs Handle Toil:
1. **Automate** – Write scripts or set up automation pipelines to handle repetitive tasks.
2. **Eliminate** – Improve the system to remove the root cause of the toil.
3. **Reduce** – Optimize processes to minimize the time spent on toil.
4. **Limit Toil** – SRE teams at Google aim to keep toil at less than 50% of
   their time to allow more focus on strategic improvements.

## Prometheus
- Prometheus is an open-source tool designed for monitoring and alerting.
- It works by scraping metrics from configured endpoints at specific intervals and storing them in a time-series database.
- Collecting metrics like CPU usage, memory, network traffic, and custom app metrics.

## Grafana
- Grafana is an open-source platform used to visualize data collected by Prometheus (or other data sources).
- It creates interactive and real-time dashboards with metrics from Prometheus.
- Supports multiple data sources (Prometheus, Loki, InfluxDB, MySQL, Elasticsearch, etc.).
- By using Grafana, we can create real-time dashboards to monitor system health and performance.

## Loki
- Loki is a log aggregation and querying tool.
- Loki is a log aggregation system from Grafana Labs, designed to work similarly to Prometheus but for logs.
- Unlike traditional log management systems, Loki doesn’t index the log content but instead indexes labels.
- Correlating logs with Prometheus metrics for debugging.

## Running the Grafana Dashboard Script
1. Firstly, it asks whether to reset the minikube or not. If we select yes then it stops and deletes the existing minikube cluster.
2. In the script, we have mentioned to start minikube:
```bash
minikube start --driver=docker --cpus=2 --memory=3072
```
3. Next created a `sample-app.yaml` file, where it is having a container with three types of messages:
   - **[INFO]** – Regular messages
   - **[DEBUG]** – Debugging messages
   - **[ERROR]** – Random error messages
4. Next, we need to install Prometheus, but before that delete the namespace if any exists.
5. Installing Prometheus through Helm.

### Helm
- Helm is Kubernetes package manager.
- Helm helps us to deploy and manage Kubernetes applications using Helm charts.
- Helm charts simplify the installation and management of complex applications like Prometheus, Grafana, and Loki.

6. There is a file namely `values.yaml`, where it is used to adjust the settings of Prometheus. Helm reads the `values.yaml` file and sets up Prometheus with the specified settings.
7. After that, waiting for Prometheus to start using the code:
```bash
kubectl wait --for=condition=ready pod --selector="app.kubernetes.io/name=prometheus,app.kubernetes.io/component=server" -n monitoring --timeout=120s
```
8. It ensures that Prometheus is ready to go to the next step.
9. Next, we installed Loki-stack from the Grafana chart.
10. We have created `values.yaml` file, it consists of:
   - admin, password
   - datasources including URL
   - dashboard providers and preload it
11. Installing the Grafana chart through Helm and providing the `values.yaml` file as well.
12. After that exposing Grafana and forwarding the port.
13. We have mentioned the custom dashboard using JSON format, but we haven't used it. We created dashboards using UI.
14. After that run the script using the command:
```bash
./simple-grafana-monitoring.sh
```
15. Commands that made me execute this file:
```bash
sudo dockerd &
minikube delete
minikube start --driver=docker --cpus=2 --memory=2048
minikube status
```
```bash
kubectl get nodes
```
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh
```

16. Next deleted the kubectl monitoring namespace.
17. Finally executed the file again.
![Linux Commands](https://github.com/user-attachments/assets/249e62e9-c5f5-40d4-a3c7-3c2d785cc961)
![Linux Commands](https://github.com/user-attachments/assets/0c174c06-ca82-4308-9034-f6125fa2ec2d)
![Linux Commands](https://github.com/user-attachments/assets/4a34d196-5a78-433c-bdc4-e645422d174d)
![Linux Commands](https://github.com/user-attachments/assets/b944ed4e-af47-4c8d-903f-0b15b23dc830)

## Login by admin as password and username
<img width="434" alt="image" src="https://github.com/user-attachments/assets/f976e4cd-3d98-41c7-9ac5-e478d3ca919e" />

## Creating the Grafana Dashboard
1. Open Grafana using the exposed URL.
2. Login with username and password.
3. Create a new dashboard in Grafana.
4. Select **Loki** as a data source.
5. From the visualization option, select **"Logs"**.
6. Set the panel title and click on apply.
7. In label filters, select **namespace** and give **sample-app** there. In **Line Contains** tab, give the word **ERROR**.
<img width="527" alt="image" src="https://github.com/user-attachments/assets/6f17efe3-70f5-4d82-82e3-3133529d71f9" />
<img width="949" alt="image" src="https://github.com/user-attachments/assets/6329b790-6202-4b40-ae96-622a1c15325a" />
<img width="950" alt="image" src="https://github.com/user-attachments/assets/493c302b-9749-4fe4-bc2d-bd3b7daf6a2c" />

## Creating CPU Usage Panel
1. Select **Prometheus** as a data source.
2. Switch from builder to code mode.
3. Execute the below query:
```promql
sum(rate(container_cpu_usage_seconds_total{namespace="sample-app"}[5m])) by (pod)
```
4. The query returns the average CPU usage rate (in seconds per second) for each pod within the sample-app namespace over the last 5 minutes.
5. Set the unit to **Percent (0-100)**.


## Creating Log Volume Panel
1. Created another panel in the same dashboard called **Log Volume by Pod**.
2. Executed the query below:
```promql
sum(count_over_time({namespace="sample-app"}[5m])) by (pod_name)
```
3. Configured dashboard settings and saved the dashboard.
4. Dashboard now shows 3 panels.
