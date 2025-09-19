# Hysteria

```{.yaml linenums="1"}
proxies:
- name: "hysteria"
  type: hysteria
  server: server.com
  port: 443
  # ports: 1000,2000-3000,4000 # port 不可省略
  auth-str: yourpassword
  # obfs: obfs_str
  # alpn:
  #   - h3
  protocol: udp # 支持 udp/wechat-video/faketcp
  up: "30 Mbps" # 若不写单位,默认为 Mbps
  down: "200 Mbps" # 若不写单位,默认为 Mbps
  # sni: server.com
  # skip-cert-verify: false
  # recv-window-conn: 12582912
  # recv-window: 52428800
  # disable_mtu_discovery: false
  # fingerprint: xxxx # 配置指纹将实现 SSL Pining 效果, 可使用 openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem 获取
  # fast-open: true # 启用 Fast Open (降低连接建立延迟),默认为 false
```

[通用字段](./index.md)
