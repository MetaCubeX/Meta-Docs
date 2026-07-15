# AnyTLS

```{.yaml linenums="1"}
listeners:
- name: anytls-in-1
  type: anytls
  port: 10818
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
# "shadow-tls"、"res-tls" 和 "jls-config" 均未启用且 "allow-insecure" 不为 true 时，必须填写 "certificate" 和 "private-key"；启用 ShadowTLS、ResTLS 或 JLS 时不要填写
  users:
    username1: password1
    username2: password2
  certificate: ./server.crt # 证书 PEM 格式，或者 证书的路径
  private-key: ./server.key # 证书对应的私钥 PEM 格式，或者私钥路径
  # 下面两项为mTLS配置项，如果client-auth-type设置为 "verify-if-given" 或 "require-and-verify" 则client-auth-cert必须不为空
  # client-auth-type: "" # 可选值：""、"request"、"require-any"、"verify-if-given"、"require-and-verify"
  # client-auth-cert: string # 证书 PEM 格式，或者 证书的路径
  # 如果填写则开启ech（可由 mihomo generate ech-keypair <明文域名> 生成）
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # shadow-tls:
    #   enable: true
    #   version: 3 # 支持 v1/v2/v3
    #   # password: shadow-tls-password # v2 配置项
    #   users: # v3 配置项
    #     - name: shadow-tls-user
    #       password: shadow-tls-password
    #   handshake:
    #     dest: www.example.com:443
    #     # proxy: ""
  # res-tls:
    #   enable: true
    #   dest: www.example.com:443
    #   password: restls-password
    #   # restls-script: ""
    #   # min-record-len: 0
    #   # proxy: ""
  # jls-config: # JLS 替代普通 TLS；未认证连接回落到 dest
    #   enable: true
    #   users:
    #     - username: jls-user
    #       password: jls-password
    #   dest: www.example.com:443
    #   # sni: www.example.com # 留空时从 dest 推导
    #   # alpn: [h2, http/1.1]
    #   # proxy: ""
    #   # rate-limit: 0 # fallback 转发限速，单位 bit/s；0 表示不限速
    ### 注意，anytls listener, 如果 "allow-insecure" 不为 true, 至少需要填写 “certificate和private-key” 或 “jls-config” 的其中一项 ###
  ### 注意，anytls listener, 如果 "allow-insecure" 不为 true, 必须填写 “certificate和private-key” ###
  # allow-insecure: false # 是否允许不开启tls加密（注意：仅用于有 nginx, caddy 前置的情况）
  padding-scheme: "" # https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
```

[通用字段](./index.md)

!!! warning ""
    `certificate` 和 `private-key` 是必要的，除非 `allow-insecure: true`（注意：仅用于有 nginx, caddy 前置的情况）

## padding-scheme

参阅 https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
