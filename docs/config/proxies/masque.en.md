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
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
  # sni: example.com
  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy.
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
  # congestion-controller: bbr # Disabled by default
  # bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  # handshake-timeout: 30

# masque-h3-l4proxy
- name: "masque-h3-l4proxy"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  udp: false # l4proxy mode currently does not support UDP
  network: h3-l4proxy
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ tls://1.1.1.1, tls://1.0.0.1 ] # Effective only when remote-dns-resolve is true
  # congestion-controller: bbr # Disabled by default
  # handshake-timeout: 30

# masque-h2
- name: "masque-h2"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
  # sni: example.com
  network: h2
  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy.
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
  # handshake-timeout: 30
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

Optional. Defaults to `quic`. For `masque-h2`, set it to `h2`. For h3-l4proxy mode, set it to `h3-l4proxy`.

!!! note
    `h3-l4proxy` mode currently does not support UDP.

## handshake-timeout

Handshake timeout in seconds. The default value is `0`, meaning only the outer connection timeout is used.
