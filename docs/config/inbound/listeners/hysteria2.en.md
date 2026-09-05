# Hysteria2

```{.yaml linenums="1"}
listeners:
- name: hy-in
  type: hysteria2
  port: 8443
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  users:
    user1: password1
    user2: password2
  up: 1000
  down: 1000
  ignore-client-bandwidth: false
  obfs: salamander
  obfs-password: password
  masquerade: ""
  #  bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  #  realm-opts:
  #    enable: true # Must be enabled manually
  #    server-url: https://realm.hy2.io
  #    token: public
  #    realm-id: my-cabin-1f3a8c2e9b
  #    stun-servers:
  #      - stun.nextcloud.com:3478
  #      - stun.sip.us:3478
  #      - global.stun.twilio.com:3478
  #    # proxy: DIRECT # Proxy used to connect to server-url
  #    # TLS options for server-url can be configured below: sni, skip-cert-verify, name-cert-verify, fingerprint, certificate, private-key, alpn
  #    # skip-cert-verify: false
  #    # name-cert-verify: example.com
  #    # ......
  alpn:
  - h3
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
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # Mbps by default
  #     down: 1000
```

## [General Fields](./index.md)

## Protocol Configuration

### users

Hysteria users and authentication passwords in `username: password` format.

!!! note ""
    The username does not participate in authentication. It is only used to match [inbound rules](../../rules/index.md#in-user).

### up/down

Hysteria bandwidth settings, in Mbps by default. See the [Hysteria documentation](https://v2.hysteria.network/docs/advanced/Full-Server-Config/#bandwidth) for details.

### ignore-client-bandwidth

Available values: `true/false`.

When enabled, the server ignores bandwidth values set by clients and always uses the traditional congestion control algorithm (BBR).

### obfs

QUIC traffic obfuscator. The only supported value is `salamander`; leave it empty to disable obfuscation.

!!! warning ""
    Enabling obfuscation makes the server incompatible with standard QUIC connections and removes its ability to masquerade as HTTP/3.

### obfs-password

Password for the QUIC traffic obfuscator.

### masquerade

Masquerades as HTTP/3 traffic. Only `file` and `http/https` are supported. When empty, the server always returns 404 Not Found.

See the [Hysteria documentation](https://v2.hysteria.network/docs/advanced/Full-Server-Config/#masquerade) for details.

|              | Example                   | Description   |
|--------------|---------------------------|---------------|
| `file`       | `file:///var/www`         | File server   |
| `http/https` | `http://127.0.0.1:8080`   | Reverse proxy |

### realm-opts

### alpn

TLS application-layer protocol negotiation list. The client must use one of the server's ALPN values or authentication will fail. Defaults to `h3`.

### certificate/private-key

Paths to the TLS certificate files.
