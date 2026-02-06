# Hysteria2

[配置参考](https://hysteria.network/zh/docs/advanced-usage/#%e5%ae%a2%e6%88%b7%e7%ab%af)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443
  ports: 443-8443
  hop-interval: 30
  password: yourpassword
  up: "30 Mbps"
  down: "200 Mbps"
  obfs: salamander # 默认为空，如果填写则开启obfs，目前仅支持salamander
  obfs-password: yourpassword

  sni: server.com
  skip-cert-verify: false
  fingerprint: xxxx # 配置指纹将实现 SSL Pining 效果, 可使用 openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem 获取
  alpn:
    - h3
  ###quic-go特殊配置项，不要随意修改除非你知道你在干什么###
  # initial-stream-receive-window： 8388608
  # max-stream-receive-window： 8388608
  # initial-connection-receive-window： 20971520
  # max-connection-receive-window： 20971520
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## ports

配置则启用端口跳跃，忽略`port`，格式参考[端口范围](../../handbook/syntax.md#_14)

## hop-interval

端口跳跃的间隔，单位为秒，默认为 30

## password

认证密码

## up/down

brutal 速率控制，若不写单位，默认为 Mbps

## obfs

QUIC 流量混淆器类型，仅可设为 `salamander`，如果为空则禁用

## obfs-password

QUIC 流量混淆器密码
