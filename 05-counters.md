# Counters

Counters are a different type of metric (compared to gauges).

A counter is a cumulative metric whose value can stay the same, increase or be reset to zero. For example, you can use a counter to represent the number of requests served, tasks completed, or errors.

Graph the 2xx requests for Grafana using `requests{app="grafana-paas", status_range="2xx", job="observe-paas-prometheus-exporter"}`. Extend the time period of your graph to spot the counter resets.

One example of when a counter reset might happen would be if your application crashed and loses the value of the counter. To mitigate counter resets when querying/graphing you will need to use function such as `increase` or `rate`.

For example, you often you want to know how many 2xx requests you've had over a certain time period. For this you should the `increase` function.

## Exercise

Use the console to give you the increase of 2xx `requests` for Grafana in the last minute using `increase(requests{app="grafana-paas", status_range="2xx"}[1m])`. Now graph this query.

------

Use the console to give you the increase of 2xx `requests` for Grafana in the last hour and then graph the query. Compare this graph with the per minute graph.

<details>
  <summary>ANSWER</summary><p>

  ```increase(requests{app="grafana-paas", status_range="2xx"}[1h])```

</p>
</details>


