# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: "BASE64_ENCODED_PRIVATE_KEY"
  public-key: "BASE64_ENCODED_PUBLIC_KEY"
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
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

## mtu

MTU size for the TUN device, defaults to 1280.

## udp

Whether to enable UDP support, defaults to false.

## remote-dns-resolve

Whether to enable remote DNS resolution through the MASQUE tunnel.

## dns

List of remote DNS servers, takes effect when `remote-dns-resolve` is enabled.
