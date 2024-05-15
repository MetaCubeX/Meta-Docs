# Hysteria2

[配置参考](https://hysteria.network/zh/docs/advanced-usage/#%e5%ae%a2%e6%88%b7%e7%ab%af)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443 # 固定端口
  # ports: 200/204/401-429/501-503 # 端口跳跃格式1
  # ports: "200,204,401-429,501-503" # 端口跳跃格式2
  #  up和down均不写或为0则使用BBR流控
  # up: "30 Mbps" # 若不写单位，默认为 Mbps
  # down: "200 Mbps" # 若不写单位，默认为 Mbps
  password: yourpassword
  # obfs: salamander # 默认为空，如果填写则开启obfs，目前仅支持salamander
  # obfs-password: yourpassword
  # sni: server.com
  # skip-cert-verify: false
  # fingerprint: xxxx
  # alpn:
  #   - h3
  # ca: "./my.ca"
  # ca-str: "xyz"
```

[通用字段](./index.md)
