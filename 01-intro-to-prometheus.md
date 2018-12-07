# Intro to Prometheus

## What is Prometheus

https://prometheus.io/

- monitoring tool
- Similar/competitor products would be graphite, influxdb
- time series DB
- uses /metric pages
- here is one of the metrics on the metrics page - https://observe-paas-prometheus-exporter.cloudapps.digital/metrics
- pull model for metrics rather than push model
- handles alerts but not responsible for delivery of alerts (that is alert manager)
- only basic dashboarding (use grafana for dashboarding)


## What Reliability Engineering Observe provide
- Prometheus, Alertmanager and Grafana.
- Integration with Zendesk for tickets and Pagerduty for alerts.
- Instructions for how to set up and use can be found in the Reliability Engineering docs.
- It's being used for monitoring and alerting by RE Observe, Registers, data.gov.uk and Notify
- https://prom-3.monitoring.gds-reliability.engineering/graph
