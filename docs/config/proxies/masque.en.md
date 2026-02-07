# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: "BASE64_ENCODED_PRIVATE_KEY"
  public-key: "BASE64_ENCODED_PUBLIC_KEY"
  # ip: 172.16.0.2/32
  # ipv6: fd00::2/128
  # uri: "/.well-known/masque/udp"
  # sni: server.com
  # mtu: 1280
  # udp: true
  # congestion-controller: bbr
  # cwnd: 10
  # remote-dns-resolve: true
  # dns:
  #   - 8.8.8.8
  #   - 1.1.1.1
```

[Common Fields](./index.md)

## private-key

Required, base64-encoded ECDSA private key.

## public-key

Required, base64-encoded ECDSA public key (server-side public key).

## ip

Local IPv4 address in CIDR format (e.g., 172.16.0.2/32).

## ipv6

Local IPv6 address in CIDR format (e.g., fd00::2/128).

## uri

MASQUE URI path, defaults to `/.well-known/masque/udp`.

## sni

TLS SNI (Server Name Indication), defaults to `masque`.

## mtu

MTU size for the TUN device, defaults to 1280.

## udp

Whether to enable UDP support, defaults to false.

## congestion-controller

Congestion control algorithm, available options: `cubic`/`new_reno`/`bbr`.

## cwnd

Congestion window size.

## remote-dns-resolve

Whether to enable remote DNS resolution through the MASQUE tunnel.

## dns

List of remote DNS servers, takes effect when `remote-dns-resolve` is enabled.
