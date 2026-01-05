# Unified Monitoring & Logging Stack (ELK + Prometheus)

[![Docker](https://img.shields.io/badge/Docker-24.0%2B-blue.svg?logo=docker)](https://www.docker.com/)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-8.x-f34f29?logo=elasticsearch)](https://www.elastic.co/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-e6522c?logo=prometheus)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-Visualization-F46800?logo=grafana)](https://grafana.com/)

A comprehensive, containerized solution for **Log Management** and **System Monitoring**. This project leverages Docker Compose to orchestrate the ELK Stack (Elasticsearch, Logstash, Kibana) alongside Fluent Bit for log shipping, and a separate Prometheus/Grafana stack for metric visualization.

## 🚀 Architecture
This repository is organized into two main subsystems:

1.  **Logging Stack (ELK + Fluent Bit):**
    * **Elasticsearch:** Distributed search and analytics engine
    * **Logstash:** Server-side data processing pipeline
    * **Kibana:** Data visualization dashboard for Elasticsearch
    * **Fluent Bit:** Lightweight log processor and forwarder

2.  **Monitoring Stack (Prometheus + Grafana):**
    * **Prometheus:** Systems monitoring and alerting toolkit
    * **Grafana:** Analytics and interactive visualization web application
    * **Node Exporter:** Hardware and OS metrics exporter (configured in Prometheus targets)

## 📂 Project Structure
```text
.
├── docker-compose.yml              # Main ELK Stack orchestration
├── fluent-bit/
│   └── fluent-bit.conf             # Fluent Bit configuration
├── logstash/
│   └── pipeline/
│       └── logstash.conf           # Logstash input/output pipeline
└── prometheus/
    ├── docker-compose.yml          # Monitoring Stack orchestration
    └── prometheus.yml              # Prometheus scrape configuration
```

## 🛠 Prerequisites
Before running this stack, ensure you have the following installed:
* Docker Engine (version 20.10+)
* Docker Compose (version 2.0+)

## 📦 Installation & Usage
1. Start the Logging Stack (ELK)
The main docker-compose.yml at the root handles the ELK components.
```bash
# Start Elasticsearch, Logstash, and Kibana in detached mode
docker-compose up -d
```
* Kibana UI: http://localhost:5601
* Logstash Input: Port 5044 (Beats protocol)

2. Start the Monitoring Stack (Prometheus & Grafana)
Navigate to the prometheus directory to launch the metrics stack.
```bash
cd prometheus
docker-compose up -d
```
* Prometheus UI: http://localhost:9090.
* Grafana UI: http://localhost:3000.

## ⚙️ Configuration Details
### Logstash Pipeline
Located in logstash/pipeline/logstash.conf. It is configured to:
* Input: Accept connections on port 5044.
* Output: Send processed logs to Elasticsearch at elasticsearch:9200.

### Fluent Bit
Located in fluent-bit/fluent-bit.conf.
* Currently configured to collect metrics (like CPU usage) and output them to stdout. You can modify the [OUTPUT] section to forward logs to Logstash or Elasticsearch directly.

### Prometheus Scrapers
Located in prometheus/prometheus.yml.
* The default configuration includes scraping jobs for prometheus itself and node_exporter (if running).

## 📝 Services & Ports
| Service        | Internal Port | External Port | Description              |
|----------------|---------------|---------------|--------------------------|
| Elasticsearch  | 9200          | 9200          | Search Engine API        |
| Kibana         | 5601          | 5601          | Visualization Dashboard |
| Logstash       | 5044          | 5044          | Log Ingestion (TCP)     |
| Prometheus     | 9090          | 9090          | Metric Scraper          |
| Grafana        | 3000          | 3000          | Metric Dashboard        |

## 🤝 Contributing
Contributions are welcome! Please fork this repository and submit a pull request for any enhancements or bug fixes.
1. Fork the Project
2. Create your Feature Branch (git checkout -b feature/NewFeature)
3. Commit your Changes (git commit -m 'Add some NewFeature')
4. Push to the Branch (git push origin feature/NewFeature)
5. Open a Pull Request
