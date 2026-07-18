# Hysteria2

[配置参考](https://hysteria.network/zh/docs/advanced/Full-Client-Config/)

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
  # bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  obfs: salamander # 默认为空，如果填写则开启obfs，目前支持salamander和 gecko
  obfs-password: yourpassword
  # obfs-min-packet-size: 512
  # obfs-max-packet-size: 1200
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx # 配置指纹将实现 SSL Pining 效果, 可使用 openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem 获取
  alpn:
    - h3
  # realm-opts:
  #   enable: true # 必须手动开启
  #   server-url: https://realm.hy2.io
  #   token: public
  #   realm-id: my-cabin-1f3a8c2e9b
  #   stun-servers:
  #     - stun.nextcloud.com:3478
  #     - stun.sip.us:3478
  #     - global.stun.twilio.com:3478
  #   # 下面支持填写针对server-url的TLS配置(sni, skip-cert-verify, name-cert-verify, fingerprint, certificate, private-key, alpn)
  #   # skip-cert-verify： false
  #   # name-cert-verify: example.com
  #   # ......
  ###quic-go特殊配置项，不要随意修改除非你知道你在干什么###
  # initial-stream-receive-window： 8388608
  # max-stream-receive-window： 8388608
  # initial-connection-receive-window： 20971520
  # max-connection-receive-window： 20971520
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## ports

配置则启用端口跳跃，忽略`port`，格式参考[端口范围](../../handbook/syntax.md#port-ranges)

## hop-interval

端口跳跃的间隔，单位为秒，默认为 30

支持填写"15-30"会每次随机选取其中一个值作为切换间隔，仅支持写一个范围（即不允许出现逗号）

## password

认证密码

## up/down

brutal 速率控制，若不写单位，默认为 Mbps

## obfs

QUIC 流量混淆器类型，可设为 `salamander`或`gecko`，如果为空则禁用

## obfs-password

QUIC 流量混淆器密码

## obfs-min-packet-size

最小线上数据包大小（字节）。仅限 `gecko`。

## obfs-max-packet-size

最大线上数据包大小（字节）。仅限 `gecko`。

## realm-opts
