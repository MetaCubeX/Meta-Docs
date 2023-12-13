```yaml
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

## 标准字段

### cipher

加密方法,支持:

aes-128-gcm aes-192-gcm aes-256-gcm

aes-128-cfb aes-192-cfb aes-256-cfb

aes-128-ctr aes-192-ctr aes-256-ctr

rc4-md5 chacha20-ietf xchacha20

chacha20-ietf-poly1305 xchacha20-ietf-poly1305

2022-blake3-aes-128-gcm 2022-blake3-aes-256-gcm 2022-blake3-chacha20-poly1305

### password

Shadowsocks 密码

### udp

是否使用 udp,默认 true

### udp-over-tcp

是否使用 UDP over TCP,默认 false

### udp-over-tcp-version

UDP over TCP 的协议版本，默认 2。可选值 1、2。

### SMUX

```yaml
  smux:
    enabled: false
    protocol: smux # smux/yamux/h2mux
    # max-connections: 4 # Maximum connections. Conflict with max-streams.
    # min-streams: 4 # Minimum multiplexed streams in a connection before opening a new connection. Conflict with max-streams.
    # max-streams: 0 # Maximum multiplexed streams in a connection before opening a new connection. Conflict with max-connections and min-streams.
    # padding: false # Enable padding. Requires sing-box server version 1.3-beta9 or later.
    # statistic: false # 控制是否将底层连接显示在面板中，方便打断底层连接
    # only-tcp: false # 如果设置为true, smux的设置将不会对udp生效，udp连接会直接走底层协议
```

## 插件

### plugin

插件,支持 `obfs`/`v2ray-plugin`/`shadow-tls`/`restls`

### plugin-opts

插件设置

#### obfs

```yaml
  plugin: obfs
  plugin-opts:
    mode: tls
    host: bing.com
```

#### v2ray-plugin

```yaml
  plugin: v2ray-plugin
  plugin-opts:
    mode: websocket # no QUIC now
      # tls: true # wss
      # 可使用 openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem 获取
      # 配置指纹将实现 SSL Pining 效果
      # fingerprint: xxxx
      # skip-cert-verify: true
      # host: bing.com
      # path: "/"
      # mux: true
      # headers:
      #   custom: value
      # v2ray-http-upgrade: false
```

#### shadow-tls

```yaml
  plugin: shadow-tls
  client-fingerprint: chrome
  plugin-opts:
    host: "cloud.tencent.com"
    password: "shadow_tls_password"
    version: 2 # support 1/2/3
```

#### restls

```yaml
  plugin: restls
  client-fingerprint: chrome  # 可以是chrome, ios, firefox, safari中的一个
  plugin-opts:
    host: "www.microsoft.com" # 应当是一个TLS 1.3 服务器
    password: [YOUR_RESTLS_PASSWORD]
    version-hint: "tls13"
    # Control your post-handshake traffic through restls-script
    # Hide proxy behaviors like "tls in tls". 
    # see https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md
    # 用restls剧本来控制握手后的行为，隐藏"tls in tls"等特征
    # 详情：https://github.com/3andne/restls/blob/main/Restls-Script:%20%E9%9A%90%E8%97%8F%E4%BD%A0%E7%9A%84%E4%BB%A3%E7%90%86%E8%A1%8C%E4%B8%BA.md
    restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
```
