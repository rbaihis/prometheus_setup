
# Prometheus Configuration Hooks and Labels

This document outlines the key hooks, labels, and configuration options available in the `prometheus.yaml` configuration file. It covers job definitions, relabeling, scrape configuration, and alerting rules.

## 1. Global Configurations

```yaml
global:
  scrape_interval: 15s  # Set the interval between each scrape.
  evaluation_interval: 15s  # Time interval for rule evaluation.
  scrape_timeout: 10s  # Timeout for scrapes.
```

### Fields:
- **scrape_interval**: How often targets are scraped.
- **evaluation_interval**: How often rules are evaluated.
- **scrape_timeout**: Timeout duration for scrapes.

## 2. Scrape Configurations

```yaml
scrape_configs:
  - job_name: "example"
    scrape_interval: 15s
    scrape_timeout: 5s
    metrics_path: "/metrics"
    scheme: https
    static_configs:
      - targets:
        - 'localhost:9090'
```

### Fields:
- **job_name**: Name of the scrape job.
- **scrape_interval**: Scrape interval for this job (overrides global setting).
- **scrape_timeout**: Timeout for scraping this job (overrides global setting).
- **metrics_path**: Path where metrics are exposed.
- **scheme**: HTTP or HTTPS for scraping.
- **static_configs**: Targets specified directly for scraping.

## 3. Relabel Configurations

Relabeling is a powerful mechanism that allows you to manipulate labels before storing or querying metrics.

```yaml
relabel_configs:
  - source_labels: [__address__]
    target_label: instance
    regex: '(.*):.*'
    replacement: '$1'
    action: replace
```

### Fields:
- **source_labels**: The list of labels used as inputs to the relabeling.
- **target_label**: The label that will be modified or created.
- **regex**: The regular expression used to match the value.
- **replacement**: The replacement string when the regex matches.
- **action**: The relabeling action (`replace`, `keep`, `drop`, etc.).

### Actions:
- **replace**: Replaces the value of the target label if the regex matches.
- **keep**: Only keeps metrics that match the regex.
- **drop**: Drops metrics that match the regex.
- **labelmap**: Renames labels based on a regex.
- **hashmod**: Hashes the source labels to create a new label.

## 4. Label Configurations

Labels are used to differentiate between metrics.

```yaml
labels:
  environment: "production"
  region: "us-west"
```

### Fields:
- **environment**: Custom label for environment (e.g., production, staging).
- **region**: Custom label for region or geographical location.

## 5. Alerting Configurations

Alerting is used to define alert conditions based on metric queries.

```yaml
rule_files:
  - "alert.rules"
```

### Example Alert Rule:

```yaml
groups:
  - name: example
    rules:
      - alert: InstanceDown
        expr: up == 0
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Instance {{ $labels.instance }} down"
          description: "Instance {{ $labels.instance }} has been down for more than 5 minutes."
```

### Fields:
- **alert**: Name of the alert.
- **expr**: Expression that triggers the alert (based on PromQL).
- **for**: Duration for which the condition must hold before firing the alert.
- **labels**: Custom labels to be added to the alert.
- **annotations**: Extra information added to the alert, like summaries or descriptions.

## 6. Service Discovery

Prometheus supports multiple service discovery mechanisms, such as Kubernetes, Consul, and Eureka.

### Eureka Example:

```yaml
scrape_configs:
  - job_name: "eureka"
    eureka_sd_configs:
      - server: http://localhost:8761/eureka/v2/
    relabel_configs:
      - source_labels: [__meta_eureka_instance_app]
        target_label: job
```

### Fields:
- **eureka_sd_configs**: Configuration for discovering services from Eureka.
- **server**: The Eureka server URL.
- **source_labels**: Labels derived from the Eureka instance.
- **target_label**: The Prometheus label to be replaced.

## 7. Basic Authentication & TLS

You can configure basic authentication and TLS for scraping targets.

```yaml
basic_auth:
  username: "user"
  password: "password"
tls_config:
  insecure_skip_verify: true
```

### Fields:
- **username**: Username for basic authentication.
- **password**: Password for basic authentication.
- **insecure_skip_verify**: Skips TLS certificate verification (not recommended for production).

---

This guide provides a quick overview of some of the most common configurations used in Prometheus. For more details, always refer to the [Prometheus documentation](https://prometheus.io/docs/prometheus/latest/configuration/configuration/).

