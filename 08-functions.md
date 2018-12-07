# Functions

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

Return the value of how many conduit apps are running in the PaaS (indicated by their app name including `conduit`)

```count(cpu{app=~".*conduit.*"})```
