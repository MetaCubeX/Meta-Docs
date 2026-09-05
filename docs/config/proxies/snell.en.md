# Snell

```{.yaml linenums="1"}
proxies:
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

- name: "snell-shadow-tls"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # udp: true
  # reuse: false
  # client-fingerprint: chrome
  obfs-opts:
     mode: shadow-tls
     host: bing.com
     password: "shadow_tls_password"
     version: 2
     alpn: ["h2"]

- name: "snell-restls"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # client-fingerprint: chrome
  obfs-opts:
    mode: restls
    host: bing.com
    password: "restls_password"
    version-hint: tls13

- name: "snell-jls"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # client-fingerprint: chrome
  obfs-opts:
    mode: jls
    host: bing.com
    username: jls-user
    password: "jls_password"
```

[Common Fields](./index.md)

## psk

Required. Snell pre-shared key.

## version

Snell version. Supports v1/2/3/4/5. UDP is only supported on v3/4/5.

## reuse

Optional. Supports v4/5. Default is false.

## client-fingerprint

Optional. Default is `chrome`.

## obfs-opts

Snell obfuscation settings.

### obfs-opts.mode

Snell obfuscation mode. Supports http/tls/shadow-tls/restls/jls.

### obfs-opts.host

Snell obfuscation hostname.

### obfs-opts.password

The ShadowTLS password. Required only when `shadow-tls` is used.

### obfs-opts.version

The ShadowTLS version. Supported values are v1/2/3. Required only when `shadow-tls` is used.

### obfs-opts.alpn

Supported values are `h2` and `http/1.1`. Required only when `shadow-tls` is used.

### obfs-opts.username

JLS username. Required only when `jls` is used.

### obfs-opts.version-hint

Restls TLS version hint. Available values: `tls12` / `tls13`.

### obfs-opts.restls-script

Optional Restls carrier traffic script after the handshake.
