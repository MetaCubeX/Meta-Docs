# Shadowsocks

```yaml
- name: "ss1"
  type: ss
  server: server
  port: 443
  cipher: aes-128-gcm
  password: "password"
  udp: true
  udp-over-tcp: false
  ip-version: ipv4
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

是否使用udp,默认true

### udp-over-tcp

是否使用UDP over TCP,默认false

## 插件

### obfs

### v2ray-plugin

### shadow-tls

### restls
