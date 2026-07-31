# TUIC

```{.yaml linenums="1"}
proxies:
- name: tuic
  server: www.example.com
  port: 10443
  type: tuic
  token: TOKEN
  uuid: 00000000-0000-0000-0000-000000000001
  password: PASSWORD_1
  disable-sni: true
  reduce-rtt: true
  request-timeout: 8000
  udp-relay-mode: native
```

[Common fields](./index.md)

[TLS fields](./tls.md)

## token

Required, user identifier for TUIC V4. Must not be written when using TUIC V5.

## uuid

Required, unique user identifier for TUIC V5. Must not be written when using TUIC V4.

## password

Required, user password for TUIC V5. Must not be written when using TUIC V4.

## ip

Overrides the DNS lookup result for the address set in the `server` option.

## heartbeat-interval

Interval for sending keepalive heartbeat packets, in milliseconds.

## disable-sni

Whether to disable SNI (Server Name Indication) in the TLS handshake. SNI is used to host multiple HTTPS sites on the same IP address.

## reduce-rtt

Whether to enable QUIC 0-RTT handshakes on the client. This can reduce connection setup time, but may increase replay attack risk.

## request-timeout

Timeout for establishing a connection to the TUIC proxy server, in milliseconds.

## udp-relay-mode

UDP packet relay mode. Available values: `native`/`quic`.

## congestion-controller

Congestion control algorithm. Available values: `cubic`/`new_reno`/`bbr`.

## max-udp-relay-packet-size

Maximum UDP relay packet size, in bytes.

## fast-open

Whether to enable Fast Open, which can reduce connection setup time.

## max-open-streams

Maximum number of open streams. Too many open streams may affect performance.
