# TPROXY

```{.yaml linenums="1"}
listeners:
- name: tproxy-in
  type: tproxy
  port: 7894
  listen: 0.0.0.0
  udp: true
```

## [General Fields](./index.md)

## Protocol Configuration

### udp

Whether to listen for UDP.
