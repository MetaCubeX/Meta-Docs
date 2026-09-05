# TUNNEL

```{.yaml linenums="1"}
listeners:
- name: tunnel-in
  type: tunnel
  port: 10816
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
  network: [tcp, udp]
  target: target.com
```
