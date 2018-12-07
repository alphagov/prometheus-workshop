# Operators

## Binary operators

Binary operators exist in prometheus such as `+`, `-`, `*`, `/`, `^`, `%`, for example `up * 3`.

#### Exercise


## Aggregation operators can be used to aggregate the elements of a single instant vector.

https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators

Common aggregators include `sum`, `min`, `max`, `avg`, `topk`, `bottomk`, `quantile`.

For example to find the total number of PaaS disk space currently in use for all applications we are monitoring you can use `sum(disk_bytes)`.

#### Exercise

1. Find what the maximum value for the `cpu` metric is.

2. Find the top 3 values for the `memory_utilization` metric is.

3. What is the total number of 2xx requests has the `notify-api` app in the `production` space processed in the last minute?

`sum(increase(requests{app="notify-api", status_range="2xx", space="production"}[1m]))`.

Note if agreggating counters you MUST use `increase` before aggregating otherwise counter resets aren't accounted for.
