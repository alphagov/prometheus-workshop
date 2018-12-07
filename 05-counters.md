# Counters

Counters are a different type of metric (compared to gauges).

A counter is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero on restart. For example, you can use a counter to represent the number of requests served, tasks completed, or errors.

Often you want to know how many 2xx requests you've had in the last minute or hour. For this you should the `increase` function.

```increase(requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}[1m])```

## Exercise

Graph the 2xx requests for Grafana using `requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}`. Increase the time period of your graph to spot the counter resets.

------

Graph the per minute increase for 2xx requests for Grafana using `increase(requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}[1m])`

------

Graph the per hour increase and compare this with the per minute increase.
