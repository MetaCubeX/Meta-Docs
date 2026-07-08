# TUNNEL

```{.yaml linenums="1"}
listeners:
- name: tunnel-in
  type: tunnel
  port: 10816
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
  network: [tcp, udp]
  target: target.com
```
