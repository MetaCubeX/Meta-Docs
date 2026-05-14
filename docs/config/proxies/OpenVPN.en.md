# OpenVPN

```{.yaml linenums="1"}
proxies

- name: "openvpn"
  type: openvpn
  server: vpn.example.com
  port: 1194
  proto: udp      
  # dev: tun      
  # cipher: AES-128-GCM 
  # auth: SHA256  
  ca: |
    -----BEGIN CERTIFICATE-----
    MIIB...example
    -----END CERTIFICATE-----
  cert: |
    -----BEGIN CERTIFICATE-----
    MIIB...example
    -----END CERTIFICATE-----
  key: |
    -----BEGIN PRIVATE KEY-----
    MIIE...example
    -----END PRIVATE KEY-----
  tls-crypt: |
    -----BEGIN OpenVPN Static key V1-----
    00000000000000000000000000000000
    ...
    -----END OpenVPN Static key V1-----
  udp: true
  # mtu: 1500
  # dialer-proxy: "ss1"

```

[Common Fields](./index.md)

## server

**Required.** The OpenVPN server address.

## port

**Required.** The OpenVPN server port. Defaults to `1194`.

## proto

**Optional.** Protocol type, supports `udp` or `tcp`. Defaults to `udp`.

## dev

**Optional.** The type of OpenVPN virtual network interface. Currently, only `tun` is supported. Defaults to `tun`.

## cipher

**Optional.** Encryption method. Currently, only `AES-128-GCM` is supported. Defaults to `AES-128-GCM`.

## auth

**Optional.** Authentication method. Currently, only `SHA256` is supported. Defaults to `SHA256`.

## ca

**Required.** The CA certificate content. Copy this from the `<ca>` tag in your `.ovpn` file. Do not include the `<ca>` tags themselves.

## cert

**Required.** The client certificate content. Copy this from the `<cert>` tag in your `.ovpn` file. Do not include the `<cert>` tags themselves.

## key

**Required.** The client private key content. Copy this from the `<key>` tag in your `.ovpn` file. Do not include the `<key>` tags themselves.

## tls-crypt

**Optional.** The TLS encryption key. Copy this from the `<tls-crypt>` tag in your `.ovpn` file. Do not include the `<tls-crypt>` tags themselves.

## udp

**Optional.** Whether to use the UDP protocol. `true` for UDP, `false` for TCP.

## mtu

**Optional.** Maximum Transmission Unit. Defaults to `1500`.

## dialer-proxy

**Optional.** Specifies the identifier for an outbound proxy. If provided, the OpenVPN connection will be established through this proxy.
