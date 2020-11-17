# Summaries

A summary also records observations (usually things like request durations or response sizes) however unlike a histogram, it does not put them into buckets but instead calculates quantiles.

A summary with a base metric name of `<basename>` exposes multiple time series during a scrape:

- streaming φ-quantiles (0 ≤ φ ≤ 1) of observed events, exposed as `<basename>{quantile="<φ>"}`
- the count of events that have been observed, exposed as `<basename>_count`
- the sum of events that have been observed, exposed as `<basename>_sum`

## Exercises

What was the 0.5 quantile for `prometheus_engine_query_duration_seconds{slice="inner_eval"}`?

<details>
  <summary>ANSWER</summary><p>

  ```prometheus_engine_query_duration_seconds{slice="inner_eval",quantile="0.5"}```

</p>
</details>

------

In what time did 99% of `prometheus_engine_query_duration_seconds{slice="prepare_time"}` complete?

<details>
  <summary>ANSWER</summary><p>

  ```prometheus_engine_query_duration_seconds{slice="prepare_time", quantile="0.99"}```

</p>
</details>
