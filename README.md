# docker-compose
Docker compose YAML script for monitoring stack services:
1. Grafana
Purpose: A powerful dashboard and visualization tool.
What it does: Takes data from monitoring sources (like Prometheus and Loki) and displays it in easy-to-read graphs, charts, and alerts.
Use case: You open Grafana in your browser and see the health, performance, and logs of your systems visually.


2. Prometheus
Purpose: Metrics collection and monitoring system.
What it does: Scrapes numeric data (metrics) like CPU usage, memory usage, request counts from various targets (e.g., your servers, containers).
Use case: It gathers detailed performance data which Grafana uses to create charts.


3. cAdvisor (Container Advisor)
Purpose: Container resource usage and performance analyzer.
What it does: Runs on a host and monitors running containers, collecting metrics such as CPU, memory, network usage per container.
Use case: Provides real-time container metrics that Prometheus can scrape.


4. Loki
Purpose: Log aggregation system.
What it does: Collects and stores logs (text output) from applications and containers but optimized to work well with Grafana for querying and visualization.
Use case: Lets you search and analyze your logs alongside your metrics, all in one place.
Currently not properly configured.

5. Node Exporter
Purpose: Host-level system metrics exporter.  
What it does: Exposes metrics like CPU load, memory usage, disk I/O, and network statistics of the host system.  
Use case: Prometheus scrapes Node Exporter to monitor the performance of the Docker host machine itself.  
Useful when you want visibility into the physical or virtual machine your containers are running on.

6. Promtail
Purpose: Log shipping agent.
What it does: Runs on your host or inside a container, reads local log files, formats them, and sends them to Loki.
Use case: Automatically collects logs (like from your app or system) and ships them to Loki for storage and querying.
Currently not properly configured.

7. Logger (Busybox container)
Purpose: A simple log generator for testing.
What it does: Continuously outputs fake log messages ({"message":"hello world"}) every second.
Use case: Provides sample log data for Promtail and Loki to process so you can test your logging pipeline.


How They Work Together:
- cAdvisor collects container metrics → Prometheus scrapes those metrics.  
- Node Exporter collects host metrics → Prometheus scrapes those too.  
- Logger generates logs → Promtail collects and forwards logs → Loki stores logs.  
- Grafana connects to both Prometheus (metrics) and Loki (logs) to show you unified dashboards.


Deployment instructions:
1. Run docker compose file, creates compose stack for grafana, prometheus, and cadvisor: docker-compose up -d
2. Connect prometheus to grafana, 
    a. Add new connection
    b. Add prometheus as connection type: Prometheus server url: http://<container_name>:<container_port>
       (http://prometheus:9090)

