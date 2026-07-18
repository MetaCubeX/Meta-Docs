# TUNNEL

```{.yaml linenums="1"}
listeners:
- name: tunnel-in
  type: tunnel
  port: 10816
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  network: [tcp, udp]
  target: target.com
```
