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

## Most useful docs for this workshop

https://prometheus.io/docs/prometheus/latest/querying/

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

`cpu{app="notify-api", space="production", exported_instance="0"}`

Write a query to return the current `memory_utilization` for all apps that have a name beginning with `registers` and are not running in the `sandbox` PaaS space.

`memory_utilization{app=~"registers.*", space!="sandbox"}`


## Instance and range vectors

* Instant vector - a set of time series containing a single sample for each time series, all sharing the same timestamp
* Range vector - a set of time series containing a range of data points over time for each time series

It's important to understand the difference between instant and range vectors PromQL functions can require you to provide the correct type as input.

#### Exercise

Make sure you on the `Console` tab.

Query `up{job="prometheus"}`. This is an instant vector

Query `up{job="prometheus"}[2m]`. This is a range vector. Compare the difference. Can you work out how often this target is scraped? What happens if you try and graph this query?

Query `up{job="prometheus"}` for a 2 hour range.


## Counters

Counters are a different type of metric (compared to gauges).

A counter is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero on restart. For example, you can use a counter to represent the number of requests served, tasks completed, or errors.

Often you want to know how many 2xx requests you've had in the last minute or hour. For this you should the `increase` function.

```increase(requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}[1m])```

#### Exercise

Graph the 2xx requests for Grafana using `requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}`. Increase the time period of your graph to spot the counter resets.

Graph the per minute increase for 2xx requests for Grafana:

```increase(requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}[1m])```

Then graph the per hour increase and compare this with the per minute increase.


## Binary operators

Binary operators exist in prometheus such as `+`, `-`, `*`, `/`, `^`, `%`, for example `up * 3`.

#### Exercise



## Aggregation operators can be used to aggregate the elements of a single instant vector.

https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators

Common aggregators include `sum`, `min`, `max`, `avg`, `topk`, `bottomk`, `quantile`.

For example to find the total number of PaaS disk space currently in use for all applications we are monitoring you can use `sum(disk_bytes)`.

#### Exercise

Find what the maximum value for the `cpu` metric is.

Find the top 3 values for the `memory_utilization` metric is.

What is the total number of 2xx requests has the `notify-api` app in the `production` space processed in the last minute?

`sum(increase(requests{app="notify-api", status_range="2xx", space="production"}[1m]))`.

Note if agreggating counters you MUST use `increase` before aggregating otherwise counter resets aren't accounted for.

## Aggregating by label

`sum(disk_bytes)` gives you total disk_bytes in use

`sum(disk_bytes) by (org)` gives you the total disk_bytes in use broken down by team

You can sum on any label and even multiple labels.

`sum(disk_bytes) by (org, space)`

#### Exercise

What is the average `memory_utilization` for each of the apps running in the `openregister` org in the `prod` space?

avg(memory_utilization{org="openregister", space="prod"}) by (app)

or even better

avg(memory_utilization{org="openregister", space="prod"}) without (exported_instance)

## Functions

`rate(v range-vector)` calculates the per-second average rate of increase of the time series in the range vector. Breaks in monotonicity (such as counter resets due to target restarts) are automatically adjusted for. Also, the calculation extrapolates to the ends of the time range, allowing for missed scrapes or imperfect alignment of scrape cycles with the range's time period.

The following example expression returns the per-second rate of HTTP requests as measured over the last 5 minutes, per time series in the range vector:

rate(http_requests_total{job="api-server"}[5m])

`avg_over_time(v range-vector)`: the average value of all points in the specified interval (also known as moving average)

#### Exercise

In the previous exercise you worked out the the average `memory_utilization` for each of the apps running in the `openregister` org in the `prod` space. Now return that list sorted by value.

sort(avg(memory_utilization{org="openregister", space="prod"}) by (app))

or even better

sort(avg(memory_utilization{org="openregister", space="prod"}) without (exported_instance))

What is per minute rate of 2xx requests to the `grafana-paas` app based on the last 5 minutes of data?

rate(requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}[5m]) * 60


- Histograms
- Summaries
- and/or/unless

Give context of why prometheus today and not grafana
