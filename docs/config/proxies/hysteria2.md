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
  obfs: salamander
  obfs-password: yourpassword
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx
  alpn:
    - h3
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
