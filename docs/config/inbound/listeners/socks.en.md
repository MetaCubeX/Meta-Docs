# SOCKS

```{.yaml linenums="1"}
listeners:
- name: socks-in
  type: socks
  port: 7891
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

If the users field is not filled in, it will follow the global [User Authentication](../../general.md/#user-authentication) settings. If it is filled in, the global settings will be ignored. To bypass authentication for this inbound connection, you can set users: []
