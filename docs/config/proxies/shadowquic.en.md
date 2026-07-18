# ShadowQUIC

```{.yaml linenums="1"}
proxies:
- name: shadowquic
  server: www.example.com
  port: 10443
  type: shadowquic
  username: username
  password: password
  # sni: example.com
  # alpn: [h3]
  # quic-versions: [v1] # support v1/v2, default v1
  # udp-over-stream: false # UDP over stream, default false
  # zero-rtt: false
  # keep-alive-interval: 10000
  # congestion-controller: cubic
  # up: 100 Mbps
  # down: 100 Mbps
  # cwnd: 32
  # bbr-profile: "standard"
  # max-datagram-frame-size: 1400
  # max-open-streams: 1024
  # recv-window-conn: 0
  # recv-window: 0
  # disable-mtu-discovery: false

```

[General Fields](./index.md)

[TLS Fields](./tls.md)

## username

**Required**, username used for ShadowQUIC authentication.

## password

**Required**, password used for ShadowQUIC authentication.

## sni

**Optional**, SNI used by the ShadowQUIC TLS handshake.

## alpn

**Optional**, application-layer protocol negotiation list. Defaults to `h3`.

## quic-versions

**Optional**, sets the supported QUIC versions. Available values: `v1`/`v2`. Default is `v1`.

## udp-over-stream

**Optional**, sets whether to transmit UDP over stream. Default is `false`.

## zero-rtt

**Optional**, sets whether to enable QUIC 0-RTT (Zero Round Trip Time) handshake.

!!! "warning"
    The 0-RTT path of the official server may read the user state before JLS authentication is completed (confirmed to affect v0.3.11); if you encounter connection failures or disconnections when using the official server, please disable zero-rtt on the server side.

## keep-alive-interval

**Optional**, the interval time for sending keep-alive heartbeat packets to maintain connection activity, in milliseconds.

## congestion-controller

**Optional**, sets the congestion control algorithm. Available values: `cubic`/`new_reno`/`bbr`. Default is `cubic`.

## up

**Optional**, sets the client upload speed, supports `Mbps`. Specifying this enables the Brutal congestion control algorithm; requires mihomo ShadowQUIC on both ends.

## down

**Optional**, sets the requested client download speed, supports `Mbps`, capped by the receiver's upload speed. Specifying this enables the Brutal congestion control algorithm; requires mihomo ShadowQUIC on both ends.

## cwnd

**Optional**, sets the initial congestion window size. Default is `32`.

## bbr-profile

**Optional**, sets the aggressiveness profile for the BBR algorithm. Available values: `standard`/`conservative`/`aggressive`. Only effective when `congestion-controller` is set to `bbr`.

## max-datagram-frame-size

**Optional**, sets the maximum allowed datagram frame size, in bytes. Default is `1400`.

## max-open-streams

**Optional**, sets the maximum number of concurrent streams that can be opened simultaneously. Default is `1024`.

## recv-window-conn

**Optional**, sets the connection-level receive window size. If set to `0`, the system default value will be used.

## recv-window

**Optional**, sets the stream-level receive window size. If set to `0`, the system default value will be used.

## disable-mtu-discovery

**Optional**, sets whether to disable Path MTU Discovery. If set to `true`, a fixed MTU size will be used.
