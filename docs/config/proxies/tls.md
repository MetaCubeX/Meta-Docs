# TLS 配置

```{.yaml linenums="1"}
proxies:
- name: "tls-example"
  tls: true
  sni: example.com
  servername: example.com
  fingerprint: xxx
  alpn:
  - h2
  - http/1.1
  skip-cert-verify: true
  client-fingerprint: random
  reality-opts:
    public-key: xxxx
    short-id: xxxx
```

## tls

启用 tls，仅适用于使用 `tls` 的协议，`trojan` 协议强制启用

## sni/servername

服务器名称指示，在 [`VMess`](./vmess.md)/[`VLESS`](./vless.md) 中为 `servername`，如果为空，则为 `server` 中的地址

## fingerprint

证书指纹，仅适用于使用 `tls` 的协议

## alpn

支持的应用层协议协商列表，按优先顺序排列。

如果两个对等点都支持 ALPN，则选择的协议将是此列表中的一个，如果没有相互支持的协议则连接将失败。

参阅 [Application-Layer Protocol Negotiation](https://sing-box.sagernet.org/zh/configuration/shared/tls/#alpn:~:text=Application%2DLayer%20Protocol%20Negotiation%E3%80%82)

## skip-cert-verify

跳过证书验证，仅适用于使用 `tls` 的协议

## client-fingerprint

客户端 utls 指纹，仅适用于 [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md) 协议，可选项参阅[全局客户端指纹](../general.md#_14)

## reality-opts

reality 配置，如果不为空，则启用 reality

### reality-opts.public-key

reality 服务端私钥对应的公钥

### reality-opts.short-id

服务端 short id 之一
