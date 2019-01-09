# Histograms

A histogram records observations (usually things like request durations or response sizes) and counts them in buckets.

A histogram with a base metric name of `<basename>` exposes multiple time series during a scrape:

- cumulative counters for the observation buckets, exposed as `<basename>_bucket{le="<upper inclusive bound>"}`
- the count of events that have been observed, exposed as `<basename>_count` (identical to `<basename>_bucket{le="+Inf"}` above)

Histograms are cumulative.

## Exercises

Find the histogram of `http_server_request_duration_seconds_bucket` since the last counter resets for the `rotas` job with `controller=""` and status `code="200"`. Use this histogram for the next three exercises.

How many requests took less than or equal 1s?

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="rotas", controller="", le="1", code="200"}```

</p>
</details>

------

How many requests were there in total?

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="rotas", controller="", le="+Inf", code="200"}```

  or

  ```http_server_request_duration_seconds_count{job="rotas", controller="", code="200"}```

</p>
</details>

------

How many requests took between 0.01s and 2.5s?

Hint, you will find this stack overflow post useful - https://stackoverflow.com/questions/45005524/prometheus-promql-subtract-two-gauge-metrics

<details>
  <summary>ANSWER</summary><p>

  ```http_server_request_duration_seconds_bucket{job="rotas", controller="", le="1", code="200"} - ignoring(le) http_server_request_duration_seconds_bucket{job="rotas", controller="", le="0.01", code="200"}```

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
