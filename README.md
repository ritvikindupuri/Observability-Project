# Self-Hosted Observability Stack for Docker Container Monitoring

This project documents the end-to-end process of building a production-grade monitoring and observability stack from scratch on a bare Ubuntu server. It leverages a containerized architecture managed by **Coolify** (a self-hosted PaaS) to deploy and integrate **Prometheus**, **Grafana**, **Node Exporter**, and **cAdvisor**.

The result is a powerful, real-time telemetry platform capable of monitoring both host-level and per-container metrics, providing critical insights for performance tuning, troubleshooting, and security analysis.

---
## Architecture Diagram

Here is the high-level architecture of the self-hosted monitoring solution:

```mermaid
graph TD
    subgraph User Access
        A[Grafana Web UI (Port 3000)]
        B[Prometheus UI (Port 9090)]
        C[Coolify UI (Port 8000)]
        D[cAdvisor UI (Port 8081)]
    end

    subgraph Coolify Host (Ubuntu Server)
        direction LR

        subgraph Docker Network
            cadvisor(cAdvisor<br>(8080 --> 8081)<br>Exports: CPU/Mem, Disk I/O, Net I/O)
            node_exporter(Node Exporter<br>(9100)<br>Exports: CPU/Mem, Disk I/O, Net Stats)
            prometheus(Prometheus<br>(9090)<br>Scrapes: cAdvisor, Node Exporter, Itself)
            grafana(Grafana<br>(3000)<br>Dashboards, PromQL)
            coolify(Coolify<br>(8000)<br>Manages, Deploys)
        end

        node_exporter --> prometheus
        cadvisor --> prometheus
        prometheus --> grafana
        coolify --> grafana
        coolify --> prometheus
        coolify --> cadvisor
        coolify --> node_exporter
    end

    A -- "Accesses" --> coolify
    B -- "Accesses" --> coolify
    C -- "Accesses" --> coolify
    D -- "Accesses" --> coolify

    style coolify fill:#f9f,stroke:#333,stroke-width:2px
    style prometheus fill:#ccf,stroke:#333,stroke-width:2px
    style grafana fill:#cfc,stroke:#333,stroke-width:2px
    style cadvisor fill:#ffc,stroke:#333,stroke-width:2px
    style node_exporter fill:#fcc,stroke:#333,stroke-width:2px
