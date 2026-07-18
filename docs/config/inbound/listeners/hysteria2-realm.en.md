# Hysteria2 Realm

```{.yaml linenums="1"}
listeners:
# This is an HTTP/HTTPS server for the realm-opts server-url used by self-hosted Hysteria2 inbounds and outbounds. Do not confuse it with a Hysteria2 server.
- name: hysteria2-realm-in-1
  type: hysteria2-realm
  port: 10820 # Supports the ports format, for example: 200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  token: public # Bearer token presented by Hysteria2 inbounds and outbounds through Authorization: Bearer <token>
  max-realms: 65536 # Maximum total realms (0 = unlimited)
  max-realms-per-ip: 4 # Maximum realms per client IP (0 = unlimited)
  trusted-proxy-header: "" # Header used to read the real client IP, for example X-Forwarded-For
  realm-name-pattern: "^[A-Za-z0-9][A-Za-z0-9_-]{0,63}$" # Regular expression that realm names must match
  # When configured, the realm provides HTTPS over TLS; otherwise it provides plaintext HTTP
  # certificate: ./server.crt # Certificate in PEM format or path to the certificate
  # private-key: ./server.key # Corresponding private key in PEM format or path to the private key
  # The following two options configure mTLS. client-auth-cert must be non-empty when client-auth-type is "verify-if-given" or "require-and-verify"
  # client-auth-type: "" # Available values: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # Certificate in PEM format or path to the certificate
  # When filled, enables ECH. Generate it with mihomo generate ech-keypair <plaintext-domain>
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # alpn: ["h2", "http/1.1"]
```

## [General Fields](./index.md)

## Protocol Configuration
