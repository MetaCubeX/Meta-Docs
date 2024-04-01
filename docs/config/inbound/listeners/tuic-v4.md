# TUIC V4

```{.yaml linenums="1"}
listeners:
- name: tuicv4-in
  type: tuic
  port: 10003
  listen: 0.0.0.0
  token:
    - TOKEN
  certificate: ./server.crt
  private-key: ./server.key
  congestion-controller: bbr
  max-idle-time: 15000
  authentication-timeout: 1000
  alpn:
    - h3
  max-udp-relay-packet-size: 1500
```
