# SOCKS

```{.yaml linenums="1"}
listeners:
- name: socks-in
  type: socks
  port: 7891
  listen: 0.0.0.0
  udp: true
```

## [通用字段](./index.md)

## 协议配置

### udp

是否监听 UDP

### 用户验证

使用全局 [用户验证](../../general.md/#_2)
