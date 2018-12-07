# Aggregating by label

`sum(disk_bytes)` gives you total disk_bytes in use

`sum(disk_bytes) by (org)` gives you the total disk_bytes in use broken down by team

You can sum on any label and even multiple labels.

`sum(disk_bytes) by (org, space)`

## Exercise

1. What is the average `memory_utilization` for each of the apps running in the `openregister` org in the `prod` space?

avg(memory_utilization{org="openregister", space="prod"}) by (app)

or even better

avg(memory_utilization{org="openregister", space="prod"}) without (exported_instance)


