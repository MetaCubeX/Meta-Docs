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
    #  -----BEGIN OpenVPN Static key V1-----
    #  ...
    #  -----END OpenVPN Static key V1-----
    # peer-info:
    #   IV_HWADDR: "52:54:00:ff:72:87"
    #   UV_DEVICE_ID: "laptop-001"
    # ping: 10
    # ping-restart: 60
    # handshake-timeout: 30
    # dev: tun
    # cipher: AES-128-GCM
    # data-ciphers: [AES-256-GCM, AES-128-GCM]
    # data-ciphers-fallback: AES-128-CBC
    # auth: SHA256
    # comp-lzo: "no"
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

## ping

Optional, defaults to `0`.

## ping-restart

Optional, defaults to `0`.

## peer-info

Optional key-value pairs passed to the server as peer-info. They are appended after the built-in `IV_VER`/`IV_PROTO`/`IV_CIPHERS` values and can be used by the server for admission decisions.

## handshake-timeout

Optional handshake timeout in seconds. The default value is `0`, meaning only the outer connection timeout is used.

## dev

Optional, virtual network interface type. Currently only `tun` is supported. Defaults to `tun`.

## cipher

Optional, encryption method. Supports `AES-128-GCM` / `AES-256-GCM` / `AES-128-CBC` / `AES-256-CBC` / `CHACHA20-POLY1305`. Defaults to `AES-128-GCM`. `AES-CBC` will be treated as `AES-128-CBC`.

## auth

Optional, data authentication algorithm. Supports `MD5` /`SHA1` / `SHA256`/ `SHA384` / `SHA512`. Defaults to `SHA256`. AEAD ciphers will ignore the auth configuration.

## comp-lzo

Optional, data compression method. Supported values: `yes`, `no`, `adaptive`.

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
