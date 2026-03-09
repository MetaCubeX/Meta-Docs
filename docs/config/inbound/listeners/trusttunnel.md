# TrustTunnel

```{.yaml linenums="1"}
listeners:
- name: trusttunnel-in-1
  type: trusttunnel
  port: 10821
  listen: 0.0.0.0
  # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
  # proxy: proxy # 如果不为空则直接将该入站流量交由指定 proxy 处理 (当 proxy 不为空时，这里的 proxy 名称必须合法，否则会出错)
  users:
    - username: 1
      password: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
  certificate: ./server.crt # 证书 PEM 格式，或者 证书的路径
  private-key: ./server.key # 证书对应的私钥 PEM 格式，或者私钥路径
  network: ["tcp", "udp"] # http2+http3
  congestion-controller: bbr
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
```

[通用字段](./index.md)

!!! warning ""
    `certificate` 和 `private-key` 是必要的

## network

当为空或仅包含`tcp`时，只支持HTTP2模式；当包含`udp`时，会开启HTTP3支持。

## congestion-controller

设置QUIC拥塞控制算法。
