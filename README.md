# prometheus-workshop

## Goal
- Get a basic understanding of what Prometheus is
- Be confident with basic querying so you could use it to resolve an incident

Note, we aren’t going to focus so much on setting up the monitoring but assume Prometheus is already set up and scraping your apps

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

Prom UI
- help takes you to prom website
- targets
- alerts
- graph vs console

## Your first query

`up` is a special metric added by Prometheus which represents if a /metrics page for a target can be scraped. It is a `gauge` metric meaning it's value can go up and down. 1 represents a good scrape of a target, 0 a failed scrape of a target.

#### Exercise

Query `up` in the Prometheus user interface. Swap between the console and the graph tabs. Extend the interval to 24h in the graph tab.


## Filtering with labels

The query `up` returns many timeseries. You can see how many in the top right. You can filter using labels. You can use more than one label.

`up{job="observe-paas-prometheus-exporter"}`

You can use regex for example to get all timeseries for the metric `up` whose job label ends with `exporter`:

`up{job=~".*exporter"}`

You can also use `!=`.

#### Exercise

Write a query to return the current value for the `cpu` for the first instance of the PaaS app named `notify-api` running in the PaaS `production` space.

<details><summary>ANSWER</summary>
<p>
```cpu{app="notify-api", space="production", exported_instance="0"}```
</p>
</details>


Write a query to return the current `memory_utilization` for all apps that have a name beginning with `registers` and are not running in the `sandbox` PaaS space.

<details><summary>ANSWER</summary>
```memory_utilization{app=~"registers.*", space!="sandbox"}```
</details>


## Instance and range vectors

* Instant vector - a set of time series containing a single sample for each time series, all sharing the same timestamp
* Range vector - a set of time series containing a range of data points over time for each time series

You can chose the range of data you want to look back on using `s`,`m`,`h`,`d`,`w`,`y`.

It's important to understand the difference between instant and range vectors PromQL functions can require you to provide the correct type as input.

#### Exercise

Make sure you on the `Console` tab.

Query `up{job="prometheus"}`. This is an instant vector

Query `up{job="prometheus"}[2m]`. This is a range vector. Compare the difference. Can you work out how often this target is scraped? What happens if you try and graph this query?

Query `up{job="prometheus"}` for a 1 hour range.


## Counters

Counters are a different type of metric. Unlike gauges they can only increase in value.




prom metric types
- Gauges
- Counters
- Histograms
- Summaries




Querying

- Sum/min/max/avg/count/topk/quantile
- range vector selectors [1s,m,h,d,w,y]
- Aggregate by label
- and/or/unless
- increase/rate
- absent
- predict_linear
- sort
- <aggregation>_over_time()


