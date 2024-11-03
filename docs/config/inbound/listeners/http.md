# HTTP

```{.yaml linenums="1"}
listeners:
- name: http-in
  type: http
  port: 7890
  listen: 0.0.0.0
  users:
    - username: username1
      password: password1
```

## [通用字段](./index.md)

## 协议配置

### 用户验证

如果不填写 users 项，则遵从全局 [用户验证](../../general.md/#_2) 设置，如果填写会忽略全局设置，如想跳过该入站的验证可填写 `users: []`
