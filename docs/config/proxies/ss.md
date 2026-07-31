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

[通用字段](./index.md)

## Cipher

=== "AES"
    |          方法               |                            |                            |
    |----------------------------|----------------------------|----------------------------|
    | aes-128-ctr                | aes-192-ctr                | aes-256-ctr                |
    | aes-128-cfb                | aes-192-cfb                | aes-256-cfb                |
    | aes-128-gcm                | aes-192-gcm                | aes-256-gcm                |
    | aes-128-ccm                | aes-192-ccm                | aes-256-ccm                |
    | aes-128-gcm-siv            |                            | aes-256-gcm-siv            |

=== "CHACHA"
    | 方法                           |                                |
    |--------------------------------|--------------------------------|
    | chacha20-ietf                  |                                |
    | chacha20                       | xchacha20                      |
    | chacha20-ietf-poly1305         | xchacha20-ietf-poly1305        |
    | chacha8-ietf-poly1305          | xchacha8-ietf-poly1305         |

=== "2022 Blake3"
    | 方法                                |
    |-------------------------------------|
    | 2022-blake3-aes-128-gcm             |
    | 2022-blake3-aes-256-gcm             |
    | 2022-blake3-chacha20-poly1305       |

=== "LEA"
    | 方法               |
    |--------------------|
    | lea-128-gcm        |
    | lea-192-gcm        |
    | lea-256-gcm        |

=== "其他"
    | 方法               |
    |--------------------|
    | rabbit128-poly1305|
    | aegis-128l         |
    | aegis-256          |
    | aez-384            |
    | deoxys-ii-256-128  |
    | rc4-md5            |
    | none               |

## password

Shadowsocks 密码

## udp-over-tcp

启用 UDP over TCP，默认 false

## udp-over-tcp-version

UDP over TCP 的协议版本，默认 1。可选值 1/2。

## 插件

`plugin` 指定插件类型，支持 `obfs`、`v2ray-plugin`、`gost-plugin`、`shadow-tls`、`restls`、`kcptun` 和 `jls`；`plugin-opts` 为对应插件的设置。

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

    `mode` 固定为 `websocket`，暂不支持 QUIC。

    - `tls`：启用 WSS。
    - `fingerprint`：证书的 SHA-256 指纹；可用 `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem` 获取，设置后启用 SSL pinning。
    - `skip-cert-verify`：跳过证书验证。
    - `name-cert-verify`：指定证书校验使用的域名。
    - `host`：WebSocket `Host`。
    - `path`：WebSocket 路径。
    - `mux`：启用多路复用。
    - `headers`：自定义 WebSocket 请求头。
    - `v2ray-http-upgrade`：启用 V2Ray HTTP Upgrade。

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

    - `tls`：启用 WSS。
    - `fingerprint`：证书的 SHA-256 指纹；可用 `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem` 获取，设置后启用 SSL pinning。
    - `skip-cert-verify`：跳过证书验证。
    - `name-cert-verify`：指定证书校验使用的域名。
    - `host`：WebSocket `Host`。
    - `path`：WebSocket 路径。
    - `mux`：启用多路复用。
    - `headers`：自定义 WebSocket 请求头。

=== "shadow-tls"
    ```{.yaml linenums="1"}
    plugin: shadow-tls
    client-fingerprint: chrome
    plugin-opts:
      host: "cloud.tencent.com"
      password: "shadow_tls_password"
      version: 2
    ```

    `version` 支持 `1`、`2`、`3`。

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

    - `client-fingerprint`：可选 `chrome`、`ios`、`firefox` 或 `safari`。
    - `plugin-opts.host`：应为 TLS 1.3 服务器。
    - `plugin-opts.restls-script`：控制握手后的 Restls 载体流量，用于隐藏“TLS in TLS”等代理特征；[脚本说明](https://github.com/3andne/restls/blob/main/Restls-Script:%20%E9%9A%90%E8%97%8F%E4%BD%A0%E7%9A%84%E4%BB%A3%E7%90%86%E8%A1%8C%E4%B8%BA.md)。

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

    | 字段 | 说明 |
    | --- | --- |
    | `key` | 客户端与服务端共享密钥 |
    | `crypt` | 加密方式：`aes`、`aes-128`、`aes-128-gcm`、`aes-192`、`salsa20`、`blowfish`、`twofish`、`cast5`、`3des`、`tea`、`xtea`、`xor`、`none`、`null` |
    | `mode` | 预设模式：`fast3`、`fast2`、`fast`、`normal`、`manual` |
    | `conn` | 到服务端的 UDP 连接数量 |
    | `autoexpire` | 单个 UDP 连接自动过期时间（秒）；`0` 表示禁用 |
    | `scavengettl` | 过期连接保留时间（秒） |
    | `mtu` | UDP 数据包最大传输单元 |
    | `ratelimit` | 单个 KCP 连接的最大出站速率（字节/秒）；`0` 表示禁用，也称 packet pacing |
    | `sndwnd` / `rcvwnd` | 发送/接收窗口大小（数据包数） |
    | `datashard` / `parityshard` | Reed-Solomon 纠删码的数据分片数/校验分片数 |
    | `dscp` | DSCP 值（6 位） |
    | `nocomp` | 是否禁用压缩 |
    | `acknodelay` | 收到数据包后是否立即发送 ACK |
    | `nodelay` / `interval` / `resend` | KCP no-delay 模式、更新间隔和快速重传参数 |
    | `sockbuf` | 每个 socket 的缓冲区大小（字节） |
    | `smuxver` | smux 版本：`1` 或 `2` |
    | `smuxbuf` | 总解复用缓冲区大小（字节） |
    | `framesize` | smux 最大帧大小（字节） |
    | `streambuf` | 每个 stream 的接收缓冲区大小（字节），适用于 smux v2+ |
    | `keepalive` | 心跳间隔（秒） |

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

    `plugin-opts.alpn` 可选，填写应用层协议协商列表，例如 `[h2, http/1.1]`。
