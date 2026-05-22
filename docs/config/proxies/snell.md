# Snell

```{.yaml linenums="1"}
proxies:
 - name: "snell"
  type: snell
  server: server
  port: 44046
   psk: yourpsk
   version: 5
  # udp: true
  # reuse: false
   obfs-opts:
       mode: http
       host: bing.com
```

[通用字段](./index.md)

## psk

必须，Snell 预共享密钥

## version

snell 版本，支持 v1/2/3/4/5，仅 v3/4/5 支持 udp

## reuse

可选，支持 v4/5，默认为 false

## obfs-opts

Snell 混淆设置

### obfs-opts.mode

Snell 混淆模式，支持 http/tls

### obfs-opts.host

Snell 混淆域名
