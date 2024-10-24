<h1><strong>Phase 4: Monitoring Tools Setup</strong></h1>

In this phase, I set up a monitoring system on an AWS Ubuntu 22.04 virtual machine. The setup includes Prometheus, Node Exporter, Blackbox Exporter, and Grafana for real-time monitoring of infrastructure and services.

**Tools Installed**


- **Prometheus**: Used for metrics collection and alerting.
- **Node Exporter**: Installed on the same virtual machine to collect system-level metrics.
- **Blackbox Exporter**: Configured to monitor external services and endpoints.
- **Grafana**: Used for visualization and dashboard creation to monitor metrics collected by Prometheus.


**Installation Links**

Here are the official download links for each tool:

- [Prometheus, Node Exporter & Blackbox Exporter Download Links](https://prometheus.io/download/)
- [Grafana Download Link](https://grafana.com/grafana/download)
- [Blackbox Exporter GitHub Repository](https://github.com/prometheus/blackbox_exporter)

**Adding Jenkins and Application Endpoints to Prometheus**

To enable monitoring of Jenkins and the deployed application, I modified the `prometheus.yaml` configuration file to scrape metrics from the endpoints of `node_exporter`, `jenkins` and `blackbox`

**Prometheus**
<img width="1440" alt="Screenshot 2024-10-23 at 10 41 19 PM" src="https://github.com/user-attachments/assets/182c78fc-0979-4752-a3ec-680aef3497d3">


**Grafana Dashboards**

I created two Grafana dashboards to visualize key metrics:

- **Node Exporter Dashboard**: This dashboard monitors the performance and health of Jenkins, tracking CPU usage, memory consumption, disk I/O, and more.
- **Blackbox Exporter Dashboard**: This dashboard monitors the application availability using the Prometheus Blackbox Exporter, ensuring services are up and running.

These dashboards provide real-time insights into the system's health and are configured to trigger alerts if any service becomes unavailable or if resource utilization exceeds predefined thresholds.

**Grafana Dashboards**
<img width="1440" alt="Screenshot 2024-10-23 at 10 37 26 PM" src="https://github.com/user-attachments/assets/0d7bdf9c-3711-4978-84c6-b405ed4a2ac0">


