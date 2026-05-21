# OpenVPN

```{.yaml linenums="1"}
proxies:
  - name: "openvpn"
    type: openvpn
    server: vpn.example.com
    port: 1194
    proto: udp
    # auth-user-pass mode
    username: "user"
    password: "pass"
    # Certificate mode (Alternative to the above)
    # cert: |
    #   -----BEGIN CERTIFICATE-----
    #   ...
    #   -----END CERTIFICATE-----
    # key: |
    #   -----BEGIN PRIVATE KEY-----
    #   ...
    #   -----END PRIVATE KEY-----
    ca: |
      -----BEGIN CERTIFICATE-----
      MIIB...example
      -----END CERTIFICATE-----
    # tls-crypt: |
    #   -----BEGIN OpenVPN Static key V1-----  
    #   ...
    #   -----END OpenVPN Static key V1-----
    # dev: tun
    # cipher: AES-128-GCM
    # auth: SHA256
    udp: true
    # mtu: 1500
    # dialer-proxy: "ss1"
    # remote-dns-resolve: true
    # dns: [ 1.1.1.1, 8.8.8.8 ]

```

[Common Fields](./index.md)

## server

**Required**, the address of the OpenVPN server.

## port

**Required**, the port of the OpenVPN server. Defaults to `1194`.

## proto

Optional, protocol type. Supports `udp` or `tcp`. Defaults to `udp`.

## username / password

**Optional**, the username and password required for `auth-user-pass` authentication mode.

> **Note**: Must choose between these or `cert` / `key`. Both pairs cannot be empty at the same time.

## ca

**Required**, CA certificate content. Copy this from the `<ca>` tag in your `.ovpn` file.

## cert

**Optional**, client certificate content. Copy this from the `<cert>` tag in your `.ovpn` file. Can be omitted when using username/password authentication.

## key

**Optional**, client private key content. Copy this from the `<key>` tag in your `.ovpn` file. Can be omitted when using username/password authentication.

## tls-crypt

**Optional**, TLS encryption key. Copy this from the `<tls-crypt>` tag in your `.ovpn` file; do not include the tags themselves.

## dev

Optional, virtual network interface type. Currently only `tun` is supported. Defaults to `tun`.

## cipher

Optional, encryption method. Supports `AES-128-GCM` / `AES-256-GCM` / `AES-128-CBC` / `AES-256-CBC` / `CHACHA20-POLY1305`. Defaults to `AES-128-GCM`. `AES-CBC` will be treated as `AES-128-CBC`.

## auth

Optional, data authentication algorithm. Supports `SHA1` / `SHA256`. Defaults to `SHA256`. AEAD ciphers will ignore the auth configuration.

## udp

Optional, whether to use the UDP protocol. `true` for UDP, `false` for TCP.

## mtu

Optional, Maximum Transmission Unit. Defaults to `1500`.

## dialer-proxy

Optional, specifies the identifier for an outbound proxy. If set, the OpenVPN connection will be established through the specified proxy.

## remote-dns-resolve

Optional, whether to force remote DNS resolution. Defaults to `false`.

## dns

Optional, only effective when `remote-dns-resolve` is set to `true`. Specifies a list of DNS server addresses.
