# Mieru

```{.yaml linenums="1"}
listeners:
- name: mieru-in-1
  type: mieru
  port: 10818
  listen: 0.0.0.0
  transport: TCP # 支持 TCP 或者 UDP
  users:
    username1: password1
    username2: password2
```

[通用字段](./index.md)
