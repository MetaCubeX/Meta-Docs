# TrustTunnel

```{.yaml linenums="1"}
listeners:
- name: trusttunnel-in-1
  type: trusttunnel
  port: 10821
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
  # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
  users:
    - username: 1
      password: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
  certificate: ./server.crt # Certificate in PEM format or path to the certificate
  private-key: ./server.key # Corresponding private key in PEM format or path to the private key
  network: ["tcp", "udp"] # HTTP/2 + HTTP/3
  congestion-controller: bbr
  #  bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
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
```

[General Fields](./index.md)

!!! warning ""
    `certificate` and `private-key` are required.

## network

When empty or containing only `tcp`, only HTTP/2 mode is supported. Including `udp` enables HTTP/3 support.

## congestion-controller

Sets the QUIC congestion control algorithm.
