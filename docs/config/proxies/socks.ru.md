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

[Общие поля](./index.md)

[Поля TLS](./tls.md) 

## name-cert-verify

Необязательно. Изменяет только целевое имя DNSName для проверки сертификата, не меняя SNI.
