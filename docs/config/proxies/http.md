# HTTP

```{.yaml linenums="1"}
proxies:
- name: "http"
  type: http
  server: server
  port: 443
  # username: username
  # password: password
  # tls: true # https
  # skip-cert-verify: true
  # name-cert-verify: example.com
  # sni: custom.com
  # fingerprint: xxxx # 同 experimental.fingerprints 使用 sha256 指纹,配置协议独立的指纹,将忽略 experimental.fingerprints
  # ip-version: dual
  headers:
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## name-cert-verify

可选，仅修改证书 DNSName 校验目标，不修改 SNI
