### Phase 4: Monitoring Tools Setup

In this phase, I set up a monitoring system on an AWS Ubuntu 22.04 virtual machine. The setup includes Prometheus, Node Exporter, Blackbox Exporter, and Grafana for real-time monitoring of infrastructure and services.

#### Tools Installed

- **Prometheus**: Used for metrics collection and alerting.
- **Node Exporter**: Installed on the same virtual machine to collect system-level metrics.
- **Blackbox Exporter**: Configured to monitor external services and endpoints.
- **Grafana**: Used for visualization and dashboard creation to monitor metrics collected by Prometheus.

#### Installation Links

Here are the official download links for each tool:

- [Prometheus, Node Exporter & Blackbox Exporter Download Links](https://prometheus.io/download/)
- [Grafana Download Link](https://grafana.com/grafana/download)
- [Blackbox Exporter GitHub Repository](https://github.com/prometheus/blackbox_exporter)

#### Grafana Dashboards

I created two Grafana dashboards to visualize key metrics:

- **Node Exporter Dashboard**: This dashboard monitors the performance and health of Jenkins, tracking CPU usage, memory consumption, disk I/O, and more.
- **Blackbox Exporter Dashboard**: This dashboard monitors the application availability using the Prometheus Blackbox Exporter, ensuring services are up and running.

These dashboards provide real-time insights into the system's health and are configured to trigger alerts if any service becomes unavailable or if resource utilization exceeds predefined thresholds.

*Screenshot suggestion: A screenshot of the Node Exporter dashboard monitoring Jenkins and the Blackbox Exporter dashboard monitoring application availability.*
