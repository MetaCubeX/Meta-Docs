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
    # query-server-name: xxx.com
    jls-opts:
       username: jls-user
       password: jls-password
  tlsmirror-opts:
    primary-key: MDEyMzQ1Njc4OWFiY2RlZjAxMjM0NTY3ODlhYmNkZWY=
    explicit-nonce-ciphersuites: [
      156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171,
      172, 173, 49195, 49196, 49197, 49198, 49199, 49200, 49201, 49202, 49290, 49291,
      49293, 49316, 49317, 49318, 49319, 49320, 49321, 49322, 49323, 49324, 49325,
      49326, 49327, 52392, 52393, 52394, 52395, 52396, 52397, 52398
    ]
    defer-instance-derived-write-time:
      base-nanoseconds: 0
      uniform-random-multiplier-nanoseconds: 0
    transport-layer-padding:
      enabled: false
    connection-enrolment:
      primary-egress-outbound: ""
    sequence-watermarking-enabled: false
    embedded-traffic-generator:
      steps:
        - name: example
          host: example.com
          path: /
          method: GET
          headers:
            - name: User-Agent
              value: mihomo
            - name: Accept
              values:
                - text/html
                - application/xhtml+xml
          connection-ready: true
          connection-recall-exit: true
          h2-do-not-wait-for-download-finish: false
          wait-time:
            base-nanoseconds: 1000000000
            uniform-random-multiplier-nanoseconds: 0
          next-step:
            - weight: 1
              goto-location: 0
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
获取，也可以通过 Chrome 浏览器的“证书查看器”中“SHA256 指纹”的“证书”项获取

!!! warning

    * 当填入的是叶子证书（即包含 sni 名称的证书）时，只验证服务端发来的证书是否符合该指纹，不会做额外校验
    
    * 当填入的是其他类型的证书（如中级证书或根证书）的指纹，会验证服务端发来的证书链是否由该证书签发，自 v1.19.20 起还需符合 sni/servername 的要求
    
    * 此项中的指纹是完整证书的指纹，不是 HPKP 中定义的“证书公钥指纹”，请勿混淆

## alpn

支持的应用层协议协商列表，按优先顺序排列。

如果两个对等点都支持 ALPN，则选择的协议将是此列表中的一个，如果没有相互支持的协议则连接将失败。

参阅 [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

跳过证书验证，仅适用于使用 `tls` 的协议

## certificate

如果填写则开启 [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)（需要和 private-key 同时填写），内容为证书 PEM 格式，或者 证书的路径

## private-key

如果填写则开启 [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)（需要和 certificate 同时填写），内容为证书对应的私钥 PEM 格式，或者私钥路径

## client-fingerprint

客户端 utls 指纹，仅适用于 [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md)/[`AnyTLS`](./anytls.md) 协议

!!! note
    可选：`chrome`, `firefox`, `safari`, `ios`, `android`, `edge`, `360`, `qq`, `random`, 若选择 `random`, 则按 Cloudflare Radar 数据按概率生成一个现代浏览器指纹。

## reality-opts

reality 配置，如果不为空，则启用 reality

!!! warning
    由于 xray-core 刻意的[不兼容行为](https://github.com/XTLS/Xray-core/commit/af7eb68028732a8ee3c0e5d6ab2b8a657bb2e770)，我们不会考虑 xray v26.7.11+ 版本的兼容性，如果不能使用请更换服务端（如 [mihomo 原生 listener](../inbound/listeners/index.md)，sing-box 或旧版 xray-core），或用其他协议替代

### reality-opts.public-key

reality 服务端私钥对应的公钥

### reality-opts.short-id

服务端 short id 之一

### reality-opts.support-x25519mlkem768

支持 X25519-MLKEM768 密钥交换

## jls-opts

使用 sni 作为 JLS SNI

## ech-opts

### ech-opts.enable

启用 ECH（Encrypted Client Hello），如果为 true，则启用 ECH

### ech-opts.config

ECH 配置，如果为空则通过 dns 解析，不为空则通过该值指定，格式为经过 base64 编码的 ech 参数（例如`AEn+DQBFKwAgACABWIHUGj4u+PIggYXcR5JF0gYk3dCRioBW8uJq9H4mKAAIAAEAAQABAANAEnB1YmxpYy50bHMtZWNoLmRldgAA`）

!!! info
    您可以通过`mihomo generate ech-keypair test.com`命令为服务器端和客户端生成符合要求的自签名 ech 配置对，请将`test.com`自行替换为您想要对外展现的 SNI 域名，输出中`Config:`后的内容可填在此处，`Key:`后的内容应填在服务端的 ECH 配置（mihomo 的 listeners 中为`ech-key`）中

### ech-opts.query-server-name

可选项，不为空时用于指定通过 dns 解析时的域名

## tlsmirror-opts

当 `tls` 为 `true` 且配置 `tlsmirror-opts` 时启用 tlsmirror。tlsmirror 的 TLS 载体会使用同一出站中的 `servername`、`alpn`、`skip-cert-verify`、`fingerprint`、`certificate`、`private-key`、`client-fingerprint` 和 `ech-opts` 配置；`servername` 为空时使用 `server`。

!!! note
    目前仅 VMess 支持开启 tlsmirror，请勿在其他协议上使用

### tlsmirror-opts.primary-key

必填，32 字节主密钥的 base64 编码

### tlsmirror-opts.explicit-nonce-ciphersuites

TLS 1.2 载体使用显式 nonce 的加密套件

### tlsmirror-opts.defer-instance-derived-write-time

首次写入前的延迟

### tlsmirror-opts.transport-layer-padding

传输层填充设置

### tlsmirror-opts.connection-enrolment

v2ray 兼容的连接登记确认设置。mihomo VMess 出站一般保持 `primary-egress-outbound` 为空

#### tlsmirror-opts.connection-enrolment.primary-ingress-outbound

连接登记使用的入站侧控制出站标识，通常用于和 v2ray 兼容的服务端配置对应

#### tlsmirror-opts.connection-enrolment.primary-egress-outbound

连接登记使用的出站侧控制出站标识。mihomo VMess 出站一般保持为空；v2ray 可填写专用的控制出站 tag

### tlsmirror-opts.sequence-watermarking-enabled

是否启用序列水印

### tlsmirror-opts.embedded-traffic-generator

生成额外的 HTTP 载体流量，协议由 `alpn` 决定。未配置 `steps` 时不会启用

#### tlsmirror-opts.embedded-traffic-generator.steps

HTTP 载体流量步骤列表，按顺序执行；也可以通过 `next-step` 跳转

#### tlsmirror-opts.embedded-traffic-generator.steps.name

步骤名称，仅用于标识

#### tlsmirror-opts.embedded-traffic-generator.steps.host

HTTP 请求目标主机

#### tlsmirror-opts.embedded-traffic-generator.steps.path

HTTP 请求路径

#### tlsmirror-opts.embedded-traffic-generator.steps.method

HTTP 请求方法

#### tlsmirror-opts.embedded-traffic-generator.steps.headers

HTTP 请求头列表。每项使用 `name` 指定头名，可用 `value` 写入单个值，或用 `values` 写入多个值

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-ready

该步骤完成后再交付代理连接

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-recall-exit

代理连接关闭后退出载体流量

#### tlsmirror-opts.embedded-traffic-generator.steps.h2-do-not-wait-for-download-finish

当载体协议为 `h2` 时，不等待响应体读取完成

#### tlsmirror-opts.embedded-traffic-generator.steps.wait-time

步骤完成后等待的时间，字段同 [tlsmirror-opts.defer-instance-derived-write-time](#tlsmirror-optsdefer-instance-derived-write-time)

#### tlsmirror-opts.embedded-traffic-generator.steps.next-step

下一步骤的加权候选列表。`weight` 为权重，`goto-location` 为跳转到的步骤下标
