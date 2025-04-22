# Hysteria

```{.yaml linenums="1"}
proxies:
- name: "hysteria"
  type: hysteria
  server: server.com
  port: 443
  # ports: 1000,2000-3000,4000 # port нельзя опустить
  auth-str: yourpassword
  # obfs: obfs_str
  # alpn:
  #   - h3
  protocol: udp # поддерживает udp/wechat-video/faketcp
  up: "30 Mbps" # если единица измерения не указана, по умолчанию Mbps
  down: "200 Mbps" # если единица измерения не указана, по умолчанию Mbps
  # sni: server.com
  # skip-cert-verify: false
  # recv-window-conn: 12582912
  # recv-window: 52428800
  # ca: "./my.ca"
  # ca-str: "xyz"
  # disable_mtu_discovery: false
  # fingerprint: xxxx
  # fast-open: true # включить Fast Open (снижает задержку установки соединения), по умолчанию false
```

[Общие поля](./index.md) 