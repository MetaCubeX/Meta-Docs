# Hysteria

```{.yaml linenums="1"}
proxies:
- name: "hysteria"
  type: hysteria
  server: server.com
  port: 443
  # ports: 1000,2000-3000,4000 # port cannot be omitted
  auth-str: yourpassword
  # obfs: obfs_str
  # alpn:
  #   - h3
  protocol: udp # Supports udp/wechat-video/faketcp
  up: "30 Mbps" # Defaults to Mbps when the unit is omitted
  down: "200 Mbps" # Defaults to Mbps when the unit is omitted
  # sni: server.com
  # skip-cert-verify: false
  # recv-window-conn: 12582912
  # recv-window: 52428800
  # disable_mtu_discovery: false
  # fingerprint: xxxx # Configuring a fingerprint enables SSL pinning. You can get it with openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
  # fast-open: true # Enables Fast Open to reduce connection setup latency. Default: false
```

[Common fields](./index.md)

[TLS fields](./tls.md)
