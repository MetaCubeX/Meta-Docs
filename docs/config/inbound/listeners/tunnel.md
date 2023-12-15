```yaml
listeners:
- name: tunnel-in
  type: tunnel
  port: 10816
  listen: 0.0.0.0
  network: [tcp, udp]
  target: target.com
```
