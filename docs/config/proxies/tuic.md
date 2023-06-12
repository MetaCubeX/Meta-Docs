# TUIC

TUIC是一个轻量的基于QUIC的代理协议,由rust编写,你可以在[这里](https://github.com/EAimTY/tuic)找到更多信息

```yaml
- name: tuic
  server: www.example.com
  port: 10443
  type: tuic
  # tuicV4必须填写token （不可同时填写uuid和password）
  token: TOKEN
  # tuicV5必须填写uuid和password（不可同时填写token）
  uuid: 00000000-0000-0000-0000-000000000001
  password: PASSWORD_1
  # ip: 127.0.0.1 # for overwriting the DNS lookup result of the server address set in option 'server'
  # heartbeat-interval: 10000
  # alpn: [h3]
  disable-sni: true
  reduce-rtt: true
  request-timeout: 8000
  udp-relay-mode: native # Available: "native", "quic". Default: "native"
  # congestion-controller: bbr # Available: "cubic", "new_reno", "bbr". Default: "cubic"
  # max-udp-relay-packet-size: 1500
  # fast-open: true
  # skip-cert-verify: true
  # max-open-streams: 20 # default 100, too many open streams may hurt performance
  # sni: example.com
```
