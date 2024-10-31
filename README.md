# Full Prometheus setup 
## Docker compose based 
## Prerequirement to have it work 
  - Docker & Docker compose properly installed and configured
  - SMTP configuration auth and appPw for alert to work (ex uses gmail, use ur own credentials)
  - update the prometheus.yml to your targets (example shows nodeExporter and others as targets)
### Prometheus
  - **prometheus.yml**: (/etc/prometheus/prometheus.yml)
    - (more detail in this file)[overview_prometheusDotYaml.md]
    - global config
    - static target examples
    - pointing to alert manager
    - pointing to rules file
    - pointing to alerts file
  - **rules.yml**:
    - rules defined in it
  - **alerts.yml**:
    - alerts defined on it 
### Alertmanager
  - **alertmanager.yml** (/etc/alertmanager/alertmanager.yml)
    - global config
    - routing config to route alert to desired receiver
    - receivers defined (email sender EX)
### Grafana
  - **datasource.yml** (/etc/grafana/provisioning/datasource.yml )
    - from where grafana will be getting metrics (prometheus in this case)
  - **dashbord.yml** (/var/lib/grafana/dashboards/dashbord.yml)
    - config for dashbords to read json templates for auto generate
  - **node_exporter.json** (/var/lib/grafana/dashboards/node_exporter.json)
    - a ready template (downloaded from grafana dashbords)
