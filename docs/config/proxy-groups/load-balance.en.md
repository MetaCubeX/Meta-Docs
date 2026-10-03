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
  #hash-key: in-user
```

## Common Fields

Refer to [Common Fields](./index.md).

## strategy

Load Balancing Strategies

* `round-robin` will distribute all requests among different proxy nodes within the strategy group.

* `consistent-hashing` will assign requests with the same `target address` to the same proxy node within the strategy group.

* `sticky-sessions`: requests with the same `source address` and `target address` will be directed to the same proxy node within the strategy group, with a cache expiration of 10 minutes.

!!! note
    When the `target address` is a domain, it uses top-level domain matching.

## hash-key

Hash key, optional value `in-user`, only supported by `consistent-hashing` and `sticky-sessions`; `round-robin` does not hash, configuring it will raise an error instead of being ignored.

* `in-user` uses the authenticated inbound username as the hash key (the same field read by `IN-USER` rules). It does not change with the destination address, so a task spanning multiple domains stays on the same node and egress IP; unauthenticated requests fall back to the strategy's default key.

* When unset, the behavior is identical to previous versions.
