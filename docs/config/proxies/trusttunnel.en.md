# TrustTunnel

```{.yaml linenums="1"}
proxies:
- name: trusttunnel
  type: trusttunnel
  server: 1.2.3.4
  port: 443
  username: username
  password: password
  # client-fingerprint: chrome
  health-check: true
  udp: true
  # sni: "example.com"
  # alpn:
  #   - h2
  # skip-cert-verify: true
  # name-cert-verify: example.com
  ### quic options
  # quic: true
  # congestion-controller: bbr
  # bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  ### reuse options
  # max-connections: 8
  # min-streams: 5
  # max-streams: 0
```

[Common fields](./index.md)

[TLS fields](./tls.md)

## name-cert-verify

Optional. Only modifies the certificate's DNSName verification target, without altering the SNI.

## quic

Whether to enable QUIC. Default: `false`.

## congestion-controller

Sets the QUIC congestion control algorithm.

### max-connections

Maximum number of connections.

Conflicts with `max-streams`.

### min-streams

Minimum number of multiplexed streams in a connection before opening a new connection.

Conflicts with `max-streams`.

### max-streams

Maximum number of multiplexed streams in a connection before opening a new connection.

Conflicts with `max-connections` and `min-streams`.
