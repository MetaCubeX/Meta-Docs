# TUIC V4

```{.yaml linenums="1"}
listeners:
- name: tuicv4-in
  type: tuic
  port: 10003
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  token:
    - TOKEN
  certificate: ./server.crt # Certificate in PEM format or path to the certificate
  private-key: ./server.key # Corresponding private key in PEM format or path to the private key
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
  congestion-controller: bbr
  #  bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  max-idle-time: 15000
  authentication-timeout: 1000
  alpn:
    - h3
  max-udp-relay-packet-size: 1500
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # Mbps by default
  #     down: 1000
```
