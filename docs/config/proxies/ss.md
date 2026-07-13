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

### plugin

插件，支持 `obfs`/`v2ray-plugin`/`gost-plugin`/`shadow-tls`/`restls`/`kcptun`

### plugin-opts

插件设置

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
        mode: websocket # 目前不支持 QUIC
          # tls: true # wss
          # 您可以使用此命令获取：openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # 配置指纹（fingerprint）将启用 SSL 证书绑定（SSL pinning）
          # fingerprint: xxxx
          # skip-cert-verify: true
          # name-cert-verify: example.com # 可选，仅修改证书 DNSName 校验目标，不修改 SNI
          # host: bing.com
          # path: "/"
          # mux: true
          # headers:
          #   custom: value
          # v2ray-http-upgrade: false
    ```

=== "gost-plugin"
    ```{.yaml linenums="1"}
      plugin: gost-plugin
      plugin-opts:
        mode: websocket
          # tls: true # wss
          # 您可以使用此命令获取：openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # 配置指纹（fingerprint）将启用 SSL 证书绑定（SSL pinning）
          # fingerprint: xxxx
          # skip-cert-verify: true # 可选，仅修改证书 DNSName 校验目标，不修改 SNI
          # name-cert-verify: example.com
          # host: bing.com
          # path: "/"
          # mux: true
          # headers:
          #   custom: value
    ```

=== "shadow-tls"
    ```{.yaml linenums="1"}
      plugin: shadow-tls
      client-fingerprint: chrome
      plugin-opts:
        host: "cloud.tencent.com"
        password: "shadow_tls_password"
        version: 2 # 支持 1/2/3
    ```

=== "restls"
    ```{.yaml linenums="1"}
      plugin: restls
      client-fingerprint: chrome  # 可选值：chrome, ios, firefox, safari
      plugin-opts:
        host: "www.microsoft.com" # 应当是一个 TLS 1.3 服务器
        password: [YOUR_RESTLS_PASSWORD]
        version-hint: "tls13"
        # 通过 restls-script 控制握手后的流量行为
        # 隐藏类似 "TLS over TLS" 的代理行为
        # 详见 [https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md](https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md)
        restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
    ```

=== "kcptun"
    ```{.yaml linenums="1"}
      plugin: kcptun
      plugin-opts:
        key: it's a secrect # 客户端与服务器之间的预共享密钥（pre-shared secret）
        crypt: aes # 可选：aes, aes-128, aes-128-gcm, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, none, null
        mode: fast # 配置文件模式：fast3, fast2, fast, normal, manual
        conn: 1 # 设置连接到服务器的 UDP 连接数量
        autoexpire: 0 # 设置单个 UDP 连接的自动过期时间（秒），设为 0 表示禁用
        scavengettl: 600 # 设置已过期连接的生存时间（秒）
        mtu: 1350 # 设置 UDP 数据包的最大传输单元（MTU）
        ratelimit: 0 # 设置单个 KCP 连接的最大发送速度（字节/秒），设为 0 表示禁用。也称为数据包流控（packet pacing）
        sndwnd: 128 # 设置发送窗口大小（数据包数量）
        rcvwnd: 512 # 设置接收窗口大小（数据包数量）
        datashard: 10 # 设置里德-所罗门纠错码（Reed-Solomon） - 数据分片（datashard）
        parityshard: 3 # 设置里德-所罗门纠错码（Reed-Solomon） - 校验分片（parityshard）
        dscp: 0 # 设置 DSCP（6位）
        nocomp: false # 禁用压缩
        acknodelay: false # 收到数据包时立即刷新 ACK 确认
        nodelay: 0
        interval: 50
        resend: 0
        sockbuf: 4194304 # 每个套接字的缓冲区大小（字节）
        smuxver: 1 # 指定 smux 版本，可选 1, 2
        smuxbuf: 4194304 # 总体解复用（de-mux）缓冲区大小（字节）
        framesize: 8192 # smux 最大帧大小
        streambuf: 2097152 # 每个流的接收缓冲区大小（字节），适用于 smux v2+
        keepalive: 10 # 心跳包 (heartbeats) 发送间隔时间（秒）
     
    ```
