# TUIC V5

```{.yaml linenums="1"}
listeners:
- name: tuicv5-in
  type: tuic
  port: 10004
  listen: 0.0.0.0
  users:
    UUID1: PASSWORD1
    UUID2: PASSWORD2
  certificate: ./server.crt
  private-key: ./server.key
  congestion-controller: bbr
  max-idle-time: 15000
  authentication-timeout: 1000
  alpn:
    - h3
  max-udp-relay-packet-size: 1500
```
