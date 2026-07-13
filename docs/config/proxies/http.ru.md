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
  # fingerprint: xxxx # как в experimental.fingerprints, использует sha256 отпечаток, настраивает отпечаток независимо от протокола, игнорирует experimental.fingerprints
  # ip-version: dual
  headers:
```

[Общие поля](./index.md)

[Поля TLS](./tls.md) 

## name-cert-verify

Необязательно. Изменяет только целевое имя DNSName для проверки сертификата, не меняя SNI.

