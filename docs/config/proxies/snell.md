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

[通用字段](./index.md)

## psk

必须，Snell 预共享密钥

## version

snell 版本，支持 v1/2/3/4/5，仅 v3/4/5 支持 udp

## reuse

可选，支持 v4/5，默认为 false

## client-fingerprint

可选，默认为 chrome

## obfs-opts

Snell 混淆设置

### obfs-opts.mode

Snell 混淆模式，支持 http/tls/shadow-tls/restls/jls

### obfs-opts.host

Snell 混淆域名

### obfs-opts.password

shadow-tls密码，仅使用shadow-tls时需填写

### obfs-opts.version

shadow-tls版本，支持v1/2/3，仅使用shadow-tls时需填写

### obfs-opts.alpn

支持 h2 和 http1.1，仅使用shadow-tls时需填写

### obfs-opts.username

JLS 用户名，仅使用 jls 时需填写

### obfs-opts.version-hint

Restls TLS 版本提示，可选值为 `tls12` / `tls13`

### obfs-opts.restls-script

可选，用于控制握手后的 Restls 载体流量脚本
