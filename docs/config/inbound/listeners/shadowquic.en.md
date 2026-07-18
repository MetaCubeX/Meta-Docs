# ShadowQUIC

```{.yaml linenums="1"}
listeners:
- name: shadowquic-in-1
  type: shadowquic
  port: 10822
  listen: 0.0.0.0
  # routing-mark: 0 # Sets routing-mark for the listening socket (Linux only)
  # rule: sub-rule-name1
  # proxy: proxy
  users:
    - username: username
      password: password
  jls-upstream:
    addr: www.example.com:443
    # sni: example.com
    # proxy: proxy
    # rate-limit: 0
    # quic-version-probe: false
  # alpn:
  #   - h3
  # quic-versions: [v1]
  # zero-rtt: true
  # congestion-controller: bbr
  # up: 100 Mbps
  # down: 100 Mbps
  # ignore-client-bandwidth: false
  # cwnd: 10
  # bbr-profile: ""
  # max-idle-time: 30000
  # max-datagram-frame-size: 1400
  # recv-window-conn: 0
  # recv-window: 0
  # disable-mtu-discovery: false
```

[Common fields](./index.md)

## users

ShadowQUIC authentication users.

### users.username

Username.

### users.password

Password.

## jls-upstream

Connections that do not pass JLS authentication are forwarded to this QUIC camouflage upstream.

### jls-upstream.addr

Camouflage upstream address.

### jls-upstream.sni

JLS camouflage target SNI. If empty, it is inferred from `addr`.

### jls-upstream.proxy

Proxy used for the camouflage upstream.

### jls-upstream.rate-limit

Forwarding rate limit in bit/s. `0` means unlimited.

### jls-upstream.quic-version-probe

Probe the camouflage upstream QUIC version on the first connection.

## alpn

Application-layer protocol negotiation list.

## quic-versions

Local manual version and fallback when probing fails. Supports `v1` / `v2`.

## zero-rtt

Whether to enable 0-RTT.

## congestion-controller

Congestion control algorithm. Available values: `cubic` / `new_reno` / `bbr`. Default: `cubic`.

## up/down

Brutal bandwidth negotiation. `up` is the server upload / client download cap, and `down` is the requested server download / client upload speed.

## ignore-client-bandwidth

For Brutal. Ignore outbound `down` and use the listener `down` or automatic configuration.

## cwnd

Initial congestion window size. Default: `32`.

## bbr-profile

BBR aggressiveness profile. Available values: `standard` / `conservative` / `aggressive`. Default: `standard`.

## max-idle-time

Maximum idle time in milliseconds.

## max-datagram-frame-size

Maximum UDP datagram frame size.

## recv-window-conn

Connection-level receive window. `0` uses the default value.

## recv-window

Stream-level receive window. `0` uses the default value.

## disable-mtu-discovery

Whether to disable path MTU discovery.
