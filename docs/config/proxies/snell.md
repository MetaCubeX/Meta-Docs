# Snell

```{.yaml linenums="1"}
proxies
- name: "snell"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  version: 3
  obfs-opts:
    mode: http
    host: bing.com
```

[通用字段](./index.md)

## psk

必须，Snell 预共享密钥

## version

snell 版本，仅支持 v1-3，默认为 v1，仅 v3 支持 udp

## obfs-opts

Snell 混淆设置

### obfs-opts.mode

Snell 混淆模式，支持 http/tls

### obfs-opts.host

Snell 混淆域名
