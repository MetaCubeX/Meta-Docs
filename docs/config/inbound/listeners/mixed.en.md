# MIXED

```{.yaml linenums="1"}
listeners:
- name: mixed-in
  type: mixed
  port: 7892
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  udp: true
  users:
    - username: username1
      password: password1
  # The following two options enable TLS when set (both are required)
  # certificate: ./server.crt # Certificate in PEM format or path to the certificate
  # private-key: ./server.key # Corresponding private key in PEM format or path to the private key
  # The following two options configure mTLS. client-auth-cert must be non-empty when client-auth-type is "verify-if-given" or "require-and-verify"
  # client-auth-type: "" # Available values: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # Certificate in PEM format or path to the certificate
  # When set, enables ECH. Generate it with mihomo generate ech-keypair <plaintext-domain>
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
```

## [General Fields](./index.md)

## Protocol Configuration

### udp

Whether to listen for UDP

### User Authentication

If the users field is left empty, it will follow the global [User Authentication](../../general.md/#user-authentication) settings. If filled in, it will override the global settings. To skip authentication for this inbound connection, you can specify users: []
