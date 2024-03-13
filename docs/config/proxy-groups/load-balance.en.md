# Load-balance

```{.yaml linenums="1"}
proxy-groups:
- name: "load-balance"
  type: load-balance
  proxies:
  - ss1
  - ss2
  - vmess1
  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  #lazy: true
  #strategy: consistent-hashing # or round-robin
```

## Common Fields

Refer to [Common Fields](./index.md).

## strategy

Load balancing strategy.

* `consistent-hashing`: Distributes requests with the same top-level domain to the same proxy node within the strategy group.

* `round-robin`: Distributes all requests to different proxy nodes within the strategy group.
