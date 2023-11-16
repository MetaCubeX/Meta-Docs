```yaml
proxy-providers:
  "hy":
    type: file
    path: ./proxy_providers/hy.yaml
    health-check: {enable: true, url: http://www.gstatic.com/generate_204, interval: 300}
    override:
      down: "50 Mbps"
      up: "10 Mbps"
      dialer-proxy: proxy
      skip-cert-verify: true
      udp: true
```

目前仅支持覆写 down/up/dialer-proxy/skip-cert-verify/udp