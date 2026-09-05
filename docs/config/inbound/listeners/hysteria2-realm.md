# Hysteria2-realm

```{.yaml linenums="1"}
listeners:
# 注意，这是用于自建hysteria2入站和出站中realm-opts中server-url的HTTP/HTTPS服务器，请勿混淆
- name: hysteria2-realm-in-1
  type: hysteria2-realm
  port: 10820 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
  token: public # hysteria2入站和出站通过 `Authorization: Bearer <token>` 出示的 Bearer 令牌。
  max-realms: 65536 # maximum total realms (0 = unlimited)
  max-realms-per-ip: 4 # maximum realms per client IP (0 = unlimited)
  trusted-proxy-header: "" # header to read real client IP from (e.g. X-Forwarded-For)
  realm-name-pattern: "^[A-Za-z0-9][A-Za-z0-9_-]{0,63}$" # regex realm names must match
  # # 下面内容如果配置， realm 将通过 TLS 提供 HTTPS 服务；否则提供明文 HTTP
  # certificate: ./server.crt # 证书 PEM 格式，或者 证书的路径
  # private-key: ./server.key # 证书对应的私钥 PEM 格式，或者私钥路径
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
  # alpn: ["h2", "http/1.1"]
```

## [通用字段](./index.md)

## 协议配置
