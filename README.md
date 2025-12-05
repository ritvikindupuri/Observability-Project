# Self-Hosted Observability Stack for Docker Container Monitoring

This project documents the end-to-end process of building a production-grade monitoring and observability stack from scratch on a bare Ubuntu server. It leverages a containerized architecture managed by **Coolify** (a self-hosted PaaS) to deploy and integrate **Prometheus**, **Grafana**, **Node Exporter**, and **cAdvisor**.

The result is a powerful, real-time telemetry platform capable of monitoring both host-level and per-container metrics, providing critical insights for performance tuning, troubleshooting, and security analysis.

---
## Phase 1: Orchestrator & Monitoring Stack Deployment

The project began on a bare metal Ubuntu server. After installing Docker, the **Coolify** PaaS was deployed to act as the primary orchestrator and management plane. Using a custom Docker Compose file within Coolify, the entire monitoring stack was deployed as a set of interconnected services.

**Analysis:** The Coolify dashboard below confirms a successful deployment. It shows all core monitoring services—Prometheus (scraper), Node Exporter (host metrics), cAdvisor (container metrics), and Grafana (visualizer)—are running and managed by the orchestrator.

<img src="./assets/All Services on Coolify.png" width="800" alt="Coolify dashboard showing all running services">
*<p align="center">Figure 1: The Coolify control plane with the successfully deployed monitoring stack.</p>*

---
## Phase 2: Verification of the Data Pipeline

With the stack deployed, the next critical phase was to verify that the data pipeline was working correctly from end to end.

### Verifying the Scrape Pipeline
The first step was to check the Prometheus Targets UI. A healthy "UP" state for all jobs confirms that Prometheus is successfully connecting to and scraping metrics from both the Node Exporter and cAdvisor endpoints.

<img src="./assets/Live Exporters (Prometheus).png" width="800" alt="Prometheus Targets UI showing all jobs are UP">
*<p align="center">Figure 2: All scrape targets are healthy, confirming a successful data ingestion pipeline.</p>*

### Verifying Metric Ingestion with PromQL
The second verification step involved querying the raw data directly in Prometheus. By running a **PromQL** query for a known container metric like `container_cpu_usage_seconds_total`, I could confirm that labeled, time-series data was being successfully ingested and stored from cAdvisor.

<img src="./assets/Container CPU Metrics (Prometheus).png" width="800" alt="PromQL query showing live container CPU metrics">
*<p align="center">Figure 3: A successful PromQL query returning live, labeled telemetry from cAdvisor.</p>*

---
Phase 3: Visualization & Analysis for Security Monitoring
The final phase was to visualize the collected telemetry in Grafana to perform security-focused analysis. After adding Prometheus as a data source, I created dashboards to monitor per-container resource utilization, with a specific focus on network I/O.

Analysis: Monitoring network traffic is a core security function. The Grafana dashboard below visualizes the received and transmitted network traffic for every container on the host. This allows an analyst to establish a baseline for normal activity and immediately spot anomalies, such as a sudden, unexpected spike in outbound traffic (Sent Network Traffic), which could be a key indicator of a data exfiltration attempt.

<img src="./assets/CA-Advisor Network Traffic .png" width="800" alt="Grafana dashboard showing network traffic per container">
<p align="center">Figure 4: The final Grafana dashboard visualizing per-container network traffic for anomaly detection.</p>

---
## Conclusion

This project successfully demonstrates the creation of a comprehensive, self-hosted monitoring solution for containerized environments. By integrating a suite of industry-standard open-source tools, it provides the deep visibility necessary for maintaining system health, ensuring performance, and enabling foundational security monitoring. The skills showcased—from bare metal setup and container orchestration to data pipeline verification and final visualization—are directly applicable to modern DevOps, SRE, and cybersecurity roles where robust observability is a critical requirement.

---
##  Skills & Technologies Demonstrated

* **Containerization & Orchestration:** Docker, Docker Compose, Coolify (PaaS).
* **Monitoring & Observability:** Prometheus, Grafana, Node Exporter, cAdvisor.
* **Time-Series Databases:** Storing and querying metrics with Prometheus & PromQL.
* **Data Visualization:** Building and configuring dashboards in Grafana.
* **System Administration:** Initial server setup and tool installation on Ubuntu 22.04 LTS.
* **Network Configuration:** Managing container networks and port mappings.
