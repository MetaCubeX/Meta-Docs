# SOCKS

```{.yaml linenums="1"}
proxies:
- name: "socks"
  type: socks5
  server: server
  port: 443
  # username: username
  # password: password
  # tls: true
  # fingerprint: xxxx
  # skip-cert-verify: true
  # name-cert-verify: example.com
  # udp: true
  # ip-version: ipv6
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## name-cert-verify

可选，仅修改证书 DNSName 校验目标，不修改 SNI
