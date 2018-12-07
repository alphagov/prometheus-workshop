# Instance and range vectors

* Instant vector - a set of time series containing a single sample for each time series, all sharing the same timestamp
* Range vector - a set of time series containing a range of data points over time for each time series

It's important to understand the difference between instant and range vectors PromQL functions can require you to provide the correct type as input.

## Exercise

Make sure you on the `Console` tab.

1. Query `up{job="prometheus"}`. This is an instant vector

2. Query `up{job="prometheus"}[2m]`. This is a range vector. Compare the difference. Can you work out how often this target is scraped? What happens if you try and graph this query?

3. Query `up{job="prometheus"}` for a 2 hour range.
