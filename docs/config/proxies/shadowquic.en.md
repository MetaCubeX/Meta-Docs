# ShadowQUIC

```{.yaml linenums="1"}
proxies:
- name: shadowquic
  server: [www.example.com](https://www.example.com)
  port: 10443
  type: shadowquic
  username: username
  password: password
  # ip: 127.0.0.1
  # quic-versions: [v1]
  # udp-over-stream: false
  # zero-rtt: false
  # keep-alive-interval: 10000
  # congestion-controller: cubic
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

## ip

**Optional**, used to override the DNS lookup result of the server address set in the "server" option.

## quic-versions

**Optional**, sets the supported QUIC versions. Available values: `v1`/`v2`. Default is `v1`.

## udp-over-stream

**Optional**, sets whether to transmit UDP over stream. Default is `false`.

## zero-rtt

**Optional**, sets whether to enable QUIC 0-RTT (Zero Round Trip Time) handshake.

!!! "warning"
    The official server's 0-RTT path may read the user state before JLS authentication is completed (confirmed to affect v0.3.11); when using the official server, zero-rtt must be disabled on the server side.

## keep-alive-interval

**Optional**, the interval time for sending keep-alive heartbeat packets to maintain connection activity, in milliseconds.

## congestion-controller

**Optional**, sets the congestion control algorithm. Available values: `cubic`/`new_reno`/`bbr`. Default is `cubic`.

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
