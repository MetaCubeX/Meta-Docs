# Hysteria2

[配置参考](https://hysteria.network/zh/docs/advanced-usage/#%e5%ae%a2%e6%88%b7%e7%ab%af)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443
  ports: 443-8443
  password: yourpassword
  up: "30 Mbps"
  down: "200 Mbps"
  obfs: salamander # 默认为空，如果填写则开启obfs，目前仅支持salamander
  obfs-password: yourpassword

  sni: server.com
  skip-cert-verify: false
  fingerprint: xxxx
  alpn:
    - h3
  ca: "./my.ca"
  ca-str: "xyz"
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## ports

配置则启用端口跳跃，忽略`port`，格式参考[端口范围](../syntax.md#_13)

## password

认证密码

## up/down

brutal 速率控制，若不写单位，默认为 Mbps

## obfs

QUIC 流量混淆器类型，仅可设为 `salamander`，如果为空则禁用

## obfs-password

QUIC 流量混淆器密码
