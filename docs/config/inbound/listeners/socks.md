# SOCKS

```{.yaml linenums="1"}
listeners:
- name: socks-in
  type: socks
  port: 7891
  listen: 0.0.0.0
  udp: true
  users:
    - username: username1
      password: password1
  certificate: ./server.crt
  private-key: ./server.key
  # 如果填写则开启ech（可由 mihomo generate ech-keypair <明文域名> 生成）
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
```

## [通用字段](./index.md)

## 协议配置

### udp

是否监听 UDP

### 用户验证

如果不填写 users 项，则遵从全局 [用户验证](../../general.md/#_2) 设置，如果填写会忽略全局设置，如想跳过该入站的验证可填写 users: []
