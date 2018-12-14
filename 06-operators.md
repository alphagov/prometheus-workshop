# Operators

## Binary operators

Binary operators exist in prometheus such as `+`, `-`, `*`, `/`, `^`, `%`, for example `up * 3`. These can be useful for working out percentages.

## Aggregation operators can be used to aggregate the elements of a single instant vector.

https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators

Common aggregators include `sum`, `min`, `max`, `avg`, `topk`, `bottomk`, `quantile`.

For example to find the total number of PaaS disk space currently in use for all applications we are monitoring you can use `sum(disk_bytes)`.

## Exercise

What is the current maximum value for the `cpu` metric?

<details>
  <summary>ANSWER</summary><p>

  ```max(cpu)```

</p>
</details>

------

What is the total number of 2xx `requests` that the `notify-api` app in the `production` space has processed in the last minute?

Note if agreggating counters you MUST use `increase` before aggregating otherwise counter resets aren't accounted for.

<details>
  <summary>ANSWER</summary><p>

  ```sum(increase(requests{app="notify-api", status_range="2xx", space="production"}[1m]))```

</p>
</details>

------

BONUS: What are the top 3 highest values for the `memory_utilization` metric?

<details>
  <summary>ANSWER</summary><p>

  ```topk(3, memory_utilization)```

</p>
</details>

------

BONUS: What percentage of `requests` in the last 5 minutes to the`notify-api` app in the `production` space returned a 2xx status code?

<details>
  <summary>ANSWER</summary><p>

  ```sum(increase(requests{app="notify-api", status_range="2xx", space="production"}[5m])) / sum(increase(requests{app="notify-api", space="production"}[5m])) * 100```

</p>
</details>
