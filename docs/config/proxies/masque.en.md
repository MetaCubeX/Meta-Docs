# MASQUE

```{.yaml linenums="1"}
proxies:
# masque
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2
  ipv6: 2606:4700:110:84c0:163a:4914:a0ad:3342
  mtu: 1280
  udp: true
  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy.
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
  # congestion-controller: bbr # Disabled by default

# masque-h2
- name: "masque-h2"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2
  ipv6: 2606:4700:110:84c0:163a:4914:a0ad:3342
  mtu: 1280
  udp: true
  network: h2
  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy.
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
```

## Get MASQUE configuration

Generate MASQUE configuration using the [usque](https://github.com/Diniboy1123/usque) tool.

[Common Fields](./index.md)

## private-key

Required, base64-encoded ECDSA private key.

## public-key

Required, base64-encoded ECDSA public key (server-side public key).

!!! note
    Remove PEM header/footer markers such as `-----BEGIN PUBLIC KEY-----`, `-----END PUBLIC KEY-----`, and PEM line breaks `\n`.

## ip

Local IPv4 address in CIDR format (e.g., 172.16.0.2/32).

## ipv6

Local IPv6 address in CIDR format (e.g., fd00::2/128).

## mtu

MTU size for the TUN device, defaults to 1280.

## udp

Whether to enable UDP support, defaults to false.

## remote-dns-resolve

Whether to enable remote DNS resolution through the MASQUE tunnel.

## dns

List of remote DNS servers, takes effect when `remote-dns-resolve` is enabled.

## congestion-controller

Overrides the default congestion control algorithm. Disabled by default. Available values include `bbr`.

## network

Optional. Defaults to `quic`. For `masque-h2`, set it to `h2`.
