# TPROXY

```{.yaml linenums="1"}
listeners:
- name: tproxy-in
  type: tproxy
  port: 7894
  listen: 0.0.0.0
  udp: true
```

## [通用字段](./index.md)

## 协议配置

### udp

是否监听 UDP
