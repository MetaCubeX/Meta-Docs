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
  # fingerprint: xxxx # Uses the sha256 fingerprint like experimental.fingerprints. If configured, protocol-specific fingerprints ignore experimental.fingerprints
  # ip-version: dual
  headers:
```

[Common fields](./index.md)

[TLS fields](./tls.md)

## name-cert-verify

Optional. Only modifies the certificate's DNSName verification target, without altering the SNI.
