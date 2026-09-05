# Mieru

```{.yaml linenums="1"}
listeners:
- name: mieru-in-1
  type: mieru
  port: 10818
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
  transport: TCP # 支持 TCP 或者 UDP
  users:
    username1: password1
    username2: password2
  # 一个 base64 字符串用于微调网络行为
  # traffic-pattern: ""
  # 如果开启，且客户端不发送用户提示，代理服务器将拒绝连接
  # user-hint-is-mandatory: false
```

[通用字段](./index.md)
