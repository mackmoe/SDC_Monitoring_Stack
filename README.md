# Simple Docker Compose (SDC) Monitoring Stack
Full monitoring stack for the HP ProLiant DL365 G5 using docker compose. I altered port assignments for my own purposes, so that the docker host deploys the stack services using random host ports.

## Prometheus-Configuration
To set up Prometheus, we create three files:
	- prometheus/prometheus.yml — the actual Prometheus configuration
	- prometheus/alert.yml — alerts you want Prometheus to check
	- docker-compose.yml
This is where you'll change/set values for your deployment:
	- `scrape_configs` tells Prometheus where your applications are. 
	- `static_configs` hard-codes endpoints.
	- `rule_files` tells Prometheus where to search for the alert rules.
	- `scrape_interval` defines how often to check for new metric values.
	- `scrape_timeout` cancels the scrape in timeout <seconds>.

#### Optional but very helpful
The --web.enable-lifecycle is optional. However, you can reload configuration files (e.g. rules) without restarting Prometheus: 
```
curl -X POST http://localhost:9000/-/reload
```
---

## Grafana-Configuration
Grafana can work without any configuration files. However, we want to configure Prometheus as a data source in
	- `grafana/provisioning/datasources/prometheus_ds.yml`
The first volume points to our data source configuration. Adding the second volume (in the compose file), is used to save dashboards etc.

## AlertManager-Configuration
The Alertmanager sends alerts to various channels like Slack or E-Mail. #TODO - discord
Prometheus creates an alert if something violates a rule. Alertmanager silences and groups alerts.
Alertmanager needs a service and a volume added in the `docker-compose.yml`  

## Deploying the stack
Running the command: `docker-compose up -d` brings the stack up
