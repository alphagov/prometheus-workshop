# Functions

Prometheus provides many helpful functions for your queries. See the Prometheus docs for all the functions but here are a few which you may find most useful.

## rate

`rate(v range-vector)` calculates the per-second average rate of increase of the time series in the range vector. Breaks in monotonicity (such as counter resets due to target restarts) are automatically adjusted for.


## avg_over_time

`avg_over_time(v range-vector)`: the average value of all points in the specified interval (also known as moving average).


## Exercise

In the previous exercise you worked out the the average `memory_utilization` for each of the apps running in the `openregister` org in the `prod` space. Now return that instance vector sorted by value.

<details>
  <summary>ANSWER</summary><p>

  ```sort(avg(memory_utilization{org="openregister", space="prod"}) by (app))```

or

  ```sort(avg(memory_utilization{org="openregister", space="prod"}) without (exported_instance))```

</p>
</details>

------

What is per second rate of 2xx `requests` to the `grafana-paas` app based on the last 5 minutes of data?

<details>
  <summary>ANSWER</summary><p>

  ```rate(requests{app="grafana-paas", status_range="2xx"}[5m])```

</p>
</details>
