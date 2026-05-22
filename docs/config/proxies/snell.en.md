# Snell

```{.yaml linenums="1"}
proxies
- name: "snell"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # udp: true
  # reuse: false
  # obfs-opts:
  #   mode: http
  #   host: bing.com

```

[Common Fields](./index.md)

## psk

Required. Snell pre-shared key.

## version

Snell version. Supports v1/2/3/4/5. UDP is only supported on v3/4/5.

!!! warning 
    Although setting it to `5` is supported, Mihomo as a client (outbound) currently does not support Snell v5's QUIC Proxy Mode.

## reuse

Optional. Supports v4/5. Default is false.

## obfs-opts

Snell obfuscation settings.

### obfs-opts.mode

Snell obfuscation mode. Supports http/tls.

### obfs-opts.host

Snell obfuscation hostname.
