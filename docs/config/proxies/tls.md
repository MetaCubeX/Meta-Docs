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
  # certificate: xxxx
  # private-key: xxx
  client-fingerprint: chrome
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

!!! warning
    * 目前只保证叶子证书(即包含sni名称的证书）的证书指纹在此项中可用，如果您填入其他类型的证书（如中间证书或根证书）的指纹，为未定义行为
    * 此项中的指纹是完整证书的指纹，不是HPKP中定义的“证书公钥指纹”，请勿混淆

## alpn

支持的应用层协议协商列表，按优先顺序排列。

如果两个对等点都支持 ALPN，则选择的协议将是此列表中的一个，如果没有相互支持的协议则连接将失败。

参阅 [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

跳过证书验证，仅适用于使用 `tls` 的协议

## certificate

如果填写则开启 [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)（需要和private-key同时填写），内容为证书 PEM 格式，或者 证书的路径

## private-key

如果填写则开启 [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)（需要和certificate同时填写），内容为证书对应的私钥 PEM 格式，或者私钥路径

## client-fingerprint

客户端 utls 指纹，仅适用于 [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md)/[`AnyTLS`](./anytls.md) 协议

!!! note
    可选：`chrome`, `firefox`, `safari`, `ios`, `android`, `edge`, `360`, `qq`, `random`, 若选择 `random`, 则按 Cloudflare Radar 数据按概率生成一个现代浏览器指纹。

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

启用 ECH（Encrypted Client Hello），如果为true，则启用 ECH

### ech-opts.config

ECH 配置，如果为空则通过dns解析，不为空则通过该值指定，格式为经过base64编码的ech参数（例如`AEn+DQBFKwAgACABWIHUGj4u+PIggYXcR5JF0gYk3dCRioBW8uJq9H4mKAAIAAEAAQABAANAEnB1YmxpYy50bHMtZWNoLmRldgAA`）

!!! info
    您可以通过`mihomo generate ech-keypair test.com`命令为服务器端和客户端生成符合要求的自签名ech配置对，请将`test.com`自行替换为您想要对外展现的SNI域名，输出中`Config:`后的内容可填在此处，`Key:`后的内容应填在服务端的ECH配置（mihomo的listeners中为`ech-key`）中
