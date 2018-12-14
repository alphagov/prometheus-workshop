# Aggregating by label

`sum(disk_bytes)` gives you total disk_bytes in use

`sum(disk_bytes) by (org)` gives you the total disk_bytes in use grouped by by team

You can sum on any label and even multiple labels.

`sum(disk_bytes) by (org, space)`

## Exercise

What is the average `memory_utilization` for each of the apps running in the `openregister` org in the `prod` space?

<details>
  <summary>ANSWER</summary><p>

  ```avg(memory_utilization{org="openregister", space="prod"}) by (app)```

or

  ```avg(memory_utilization{org="openregister", space="prod"}) without (exported_instance)```

</p>
</details>

------

BONUS: Return the value of how many conduit apps are running in the PaaS (indicated by their app name including `conduit`). Hint - use a proxy metric such as `cpu` to indicate an app is running.

<details>
  <summary>ANSWER</summary><p>

  ```count(cpu{app=~".*conduit.*"})``` or similar

</p>
</details>


