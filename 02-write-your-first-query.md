# Your first query

`up` is a special metric added by Prometheus which represents if a /metrics page for a target can be scraped. It is a `gauge` metric meaning it's value can go up and down. 1 represents a good scrape of a target, 0 a failed scrape of a target.

#### Exercise

Query `up` in the Prometheus user interface. Swap between the console and the graph tabs. Extend the interval to 24h in the graph tab.

#### Docs

https://prometheus.io/docs/prometheus/latest/querying/
