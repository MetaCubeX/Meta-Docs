# Shadowsocks

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  type: ss
  server: server
  port: 443
  cipher: aes-128-gcm
  password: "password"
  udp: true
  udp-over-tcp: false
  udp-over-tcp-version: 2
  ip-version: ipv4
  plugin: obfs
  plugin-opts:
    mode: tls
  smux:
    enabled: false
```

[Common fields](./index.md)

## Cipher

=== "AES"
    | Method                     |                            |                            |
    |----------------------------|----------------------------|----------------------------|
    | aes-128-ctr                | aes-192-ctr                | aes-256-ctr                |
    | aes-128-cfb                | aes-192-cfb                | aes-256-cfb                |
    | aes-128-gcm                | aes-192-gcm                | aes-256-gcm                |
    | aes-128-ccm                | aes-192-ccm                | aes-256-ccm                |
    | aes-128-gcm-siv            |                            | aes-256-gcm-siv            |

=== "CHACHA"
    | Method                         |                                |
    |--------------------------------|--------------------------------|
    | chacha20-ietf                  |                                |
    | chacha20                       | xchacha20                      |
    | chacha20-ietf-poly1305         | xchacha20-ietf-poly1305        |
    | chacha8-ietf-poly1305          | xchacha8-ietf-poly1305         |

=== "2022 Blake3"
    | Method                              |
    |-------------------------------------|
    | 2022-blake3-aes-128-gcm             |
    | 2022-blake3-aes-256-gcm             |
    | 2022-blake3-chacha20-poly1305       |

=== "LEA"
    | Method             |
    |--------------------|
    | lea-128-gcm        |
    | lea-192-gcm        |
    | lea-256-gcm        |

=== "Other"
    | Method             |
    |--------------------|
    | rabbit128-poly1305 |
    | aegis-128l         |
    | aegis-256          |
    | aez-384            |
    | deoxys-ii-256-128  |
    | rc4-md5            |
    | none               |

## password

Shadowsocks password.

## udp-over-tcp

Enables UDP over TCP. Default: `false`.

## udp-over-tcp-version

Protocol version for UDP over TCP. Default: `1`. Available values: `1`/`2`.

## Plugin

`plugin` specifies the plugin type: `obfs`, `v2ray-plugin`, `gost-plugin`, `shadow-tls`, `restls`, `kcptun`, or `jls`. `plugin-opts` contains the selected plugin's settings.

=== "obfs"

    ```{.yaml linenums="1"}
    plugin: obfs
    plugin-opts:
      mode: tls
      host: bing.com
    ```

=== "v2ray-plugin"
    ```{.yaml linenums="1"}
    plugin: v2ray-plugin
    plugin-opts:
      mode: websocket
      tls: true
      fingerprint: xxxx
      skip-cert-verify: true
      name-cert-verify: example.com
      host: bing.com
      path: "/"
      mux: true
      headers:
        custom: value
      v2ray-http-upgrade: false
    ```

    `mode` is fixed to `websocket`; QUIC is not supported.

    - `tls`: enables WSS.
    - `fingerprint`: SHA-256 certificate fingerprint. Obtain it with `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem`; setting it enables SSL pinning.
    - `skip-cert-verify`: skips certificate verification.
    - `name-cert-verify`: domain used for certificate verification.
    - `host`: WebSocket `Host`.
    - `path`: WebSocket path.
    - `mux`: enables multiplexing.
    - `headers`: custom WebSocket request headers.
    - `v2ray-http-upgrade`: enables V2Ray HTTP Upgrade.

=== "gost-plugin"
    ```{.yaml linenums="1"}
    plugin: gost-plugin
    plugin-opts:
      mode: websocket
      tls: true
      fingerprint: xxxx
      skip-cert-verify: true
      name-cert-verify: example.com
      host: bing.com
      path: "/"
      mux: true
      headers:
        custom: value
    ```

    - `tls`: enables WSS.
    - `fingerprint`: SHA-256 certificate fingerprint. Obtain it with `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem`; setting it enables SSL pinning.
    - `skip-cert-verify`: skips certificate verification.
    - `name-cert-verify`: domain used for certificate verification.
    - `host`: WebSocket `Host`.
    - `path`: WebSocket path.
    - `mux`: enables multiplexing.
    - `headers`: custom WebSocket request headers.

=== "shadow-tls"
    ```{.yaml linenums="1"}
    plugin: shadow-tls
    client-fingerprint: chrome
    plugin-opts:
      host: "cloud.tencent.com"
      password: "shadow_tls_password"
      version: 2
    ```

    `version` supports `1`, `2`, and `3`.

=== "restls"
    ```{.yaml linenums="1"}
    plugin: restls
    client-fingerprint: chrome
    plugin-opts:
      host: "www.microsoft.com"
      password: [YOUR_RESTLS_PASSWORD]
      version-hint: "tls13"
      restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
    ```

    - `client-fingerprint`: one of `chrome`, `ios`, `firefox`, or `safari`.
    - `plugin-opts.host`: should be a TLS 1.3 server.
    - `plugin-opts.restls-script`: controls post-handshake Restls carrier traffic to hide proxy behavior such as “TLS in TLS”; see the [script documentation](https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md).

=== "kcptun"
    ```{.yaml linenums="1"}
    plugin: kcptun
    plugin-opts:
      key: it's a secrect
      crypt: aes
      mode: fast
      conn: 1
      autoexpire: 0
      scavengettl: 600
      mtu: 1350
      ratelimit: 0
      sndwnd: 128
      rcvwnd: 512
      datashard: 10
      parityshard: 3
      dscp: 0
      nocomp: false
      acknodelay: false
      nodelay: 0
      interval: 50
      resend: 0
      sockbuf: 4194304
      smuxver: 1
      smuxbuf: 4194304
      framesize: 8192
      streambuf: 2097152
      keepalive: 10
    ```

    | Field | Description |
    | --- | --- |
    | `key` | Pre-shared secret between client and server |
    | `crypt` | Cipher: `aes`, `aes-128`, `aes-128-gcm`, `aes-192`, `salsa20`, `blowfish`, `twofish`, `cast5`, `3des`, `tea`, `xtea`, `xor`, `none`, `null` |
    | `mode` | Preset profile: `fast3`, `fast2`, `fast`, `normal`, `manual` |
    | `conn` | Number of UDP connections to the server |
    | `autoexpire` | Automatic expiration for a single UDP connection in seconds; `0` disables it |
    | `scavengettl` | How long an expired connection remains alive, in seconds |
    | `mtu` | Maximum transmission unit for UDP packets |
    | `ratelimit` | Maximum outgoing rate of a KCP connection in bytes per second; `0` disables it. Also known as packet pacing |
    | `sndwnd` / `rcvwnd` | Send/receive window size in packets |
    | `datashard` / `parityshard` | Data/parity shard counts for Reed-Solomon erasure coding |
    | `dscp` | DSCP value (6 bits) |
    | `nocomp` | Disables compression |
    | `acknodelay` | Flushes acknowledgements immediately when a packet is received |
    | `nodelay` / `interval` / `resend` | KCP no-delay mode, update interval, and fast retransmission settings |
    | `sockbuf` | Per-socket buffer size in bytes |
    | `smuxver` | smux version: `1` or `2` |
    | `smuxbuf` | Overall demultiplexing buffer size in bytes |
    | `framesize` | Maximum smux frame size in bytes |
    | `streambuf` | Receive buffer size per stream in bytes, for smux v2+ |
    | `keepalive` | Seconds between heartbeats |

=== "jls"
    ```{.yaml linenums="1"}
    plugin: jls
    client-fingerprint: chrome
    plugin-opts:
      host: "www.example.com"
      username: "jls-user"
      password: "jls-password"
      alpn: [h2, http/1.1]
    ```

    `plugin-opts.alpn` is optional. It specifies the application-layer protocol list, for example `[h2, http/1.1]`.
