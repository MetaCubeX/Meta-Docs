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
  # hash-key: in-user
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

Selects what a hashing strategy derives its key from. Only takes effect when `strategy` is `consistent-hashing` or `sticky-sessions`

* `in-user` pins on the authenticated inbound user name (the same field [`IN-USER`](../rules/index.md#in-user) rules match on). It does not vary with the target address, so a single task spanning multiple domains will not switch nodes or change egress IP halfway through; unauthenticated requests fall back to the strategy's own key
* When unset, behavior is exactly the same as before

!!! warning ""
    `round-robin` does not hash anything, so configuring `hash-key` raises an error rather than being ignored

!!! note ""
    This option only changes *what value is hashed*, not how a strategy picks a node once it has the hash. The two strategies also differ in consistency duration: `consistent-hashing` is permanent, while `sticky-sessions` expires its cache after 10 minutes
