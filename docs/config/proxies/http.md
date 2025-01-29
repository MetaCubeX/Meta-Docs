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
  # sni: custom.com
  # fingerprint: xxxx # 同 experimental.fingerprints 使用 sha256 指纹,配置协议独立的指纹,将忽略 experimental.fingerprints
  # ip-version: dual
  headers:
    X-T5-Auth: "1962xxxxx709"
    User-Agent: "okhttp/3.11.0 Dalvik/2.1.0 ...... "
```

[通用字段](./index.md)

[TLS 字段](./tls.md)
