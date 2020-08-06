# Histograms

A histogram records observations (usually things like request durations or response sizes) and counts them in buckets.

A histogram with a base metric name of `<basename>` exposes multiple time series during a scrape:

- cumulative counters for the observation buckets, exposed as `<basename>_bucket{le="<upper inclusive bound>"}`
- the count of events that have been observed, exposed as `<basename>_count` (identical to `<basename>_bucket{le="+Inf"}` above)
- the sum of events that have been observed, exposed as `<basename>_sum`

Histograms are cumulative.

The `_sum` metric is useful in combination with the `_count` metric to calculate averages over all of the recorded observations in a given time range.

## Exercises

Find the histogram of `http_server_request_duration_seconds_bucket` since the last counter resets for the `pay-control-plane` job with `controller="cluster#index"` and status `code="200"`. Use this histogram for the next three exercises.



How many requests took less than or equal 1s?

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="pay-control-plane", code="200",controller="cluster#index", le="1"}```

</p>
</details>

------

How many requests were there in total?

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="pay-control-plane", code="200",controller="cluster#index", le="+Inf"}```

  or

  ```http_server_request_duration_seconds_count{job="pay-control-plane", code="200",controller="cluster#index"}```

</p>
</details>

------

How many requests took between 0.25s and 2.5s?

Hint, you will find this stack overflow post useful - https://stackoverflow.com/questions/45005524/prometheus-promql-subtract-two-gauge-metrics

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="pay-control-plane", code="200",controller="cluster#index", le="2.5"} - ignoring(le) http_server_request_duration_seconds_bucket{job="pay-control-plane", code="200",controller="cluster#index", le="0.25"}```

</p>
</details>

------

BONUS: What percentage of requests in the last hour took less than 2.5s for the `grafana-paas` app as measured by the `response_time_bucket` metric for the `observe-paas-prometheus-exporter` job?

<details>
  <summary>ANSWER</summary><p>

  ```sum(increase(response_time_bucket{job="observe-paas-prometheus-exporter", app="grafana-paas", le="2.5"}[1h])) / sum(increase(response_time_count{job="observe-paas-prometheus-exporter", app="grafana-paas"}[1h])) * 100```

  or

  ```sum(rate(response_time_bucket{job="observe-paas-prometheus-exporter", app="grafana-paas", le="2.5"}[1h])) / sum(rate(response_time_count{job="observe-paas-prometheus-exporter", app="grafana-paas"}[1h])) * 100```

  or

  ```sum(rate(response_time_bucket{job="observe-paas-prometheus-exporter", app="grafana-paas", le="2.5"}[1h])) / sum(rate(response_time_bucket{job="observe-paas-prometheus-exporter", app="grafana-paas", le="+Inf"}[1h])) * 100```

</p>
</details>
