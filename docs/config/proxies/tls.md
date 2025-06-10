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
    support-x25519mlkem768: true
  ech-opts:
    enable: true
    config: base64_encoded_config
```

## tls

启用 tls，仅适用于使用 `tls` 的协议，`trojan` 协议强制启用

## sni/servername

服务器名称指示，在 [`VMess`](./vmess.md)/[`VLESS`](./vless.md) 中为 `servername`，如果为空，则为 `server` 中的地址

## fingerprint

证书指纹，仅适用于使用 `tls` 的协议，可使用 
```bash
openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
```
获取

## alpn

支持的应用层协议协商列表，按优先顺序排列。

如果两个对等点都支持 ALPN，则选择的协议将是此列表中的一个，如果没有相互支持的协议则连接将失败。

参阅 [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

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

### reality-opts.support-x25519mlkem768

支持 X25519-MLKEM768 密钥交换

## ech-opts

### ech-opts.enable

启用 ECH（Encrypted Client Hello），如果不为空，则启用 ECH

### ech-opts.config

ECH 配置，base64 编码的配置内容

