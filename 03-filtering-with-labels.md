# Filtering with labels

The query `up` returns many timeseries. You can see how many in the top right. You can filter using labels. You can use more than one label.

`up{job="observe-paas-prometheus-exporter"}`

You can use regex for example to get all timeseries for the metric `up` whose job label ends with `exporter`:

`up{job=~".*exporter"}`

You can also use `!=`.

## Exercise

1. Write a query to return the current value for the `cpu` for the first instance of the PaaS app named `notify-api` running in the PaaS `production` space.

`cpu{app="notify-api", space="production", exported_instance="0"}`

2. Write a query to return the current `memory_utilization` for all apps that have a name beginning with `registers` and are not running in the `sandbox` PaaS space.

`memory_utilization{app=~"registers.*", space!="sandbox"}`
