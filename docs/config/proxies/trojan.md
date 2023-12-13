```yaml
  - name: "trojan"
    type: trojan
    server: server
    port: 443
    password: yourpsk
    # client-fingerprint: random # Available: "chrome","firefox","safari","random","none"
    # fingerprint: xxxx
    # udp: true
    # sni: example.com # aka server name
    # alpn:
    #   - h2
    #   - http/1.1
    # skip-cert-verify: true
```

# Trojan-grpc

```yaml
  - name: trojan-grpc
    server: server
    port: 443
    type: trojan
    password: "example"
    network: grpc
    sni: example.com
    # skip-cert-verify: true
    # fingerprint: xxxx
    udp: true
    grpc-opts:
      grpc-service-name: "example"
```

# Trojan-ws

```yaml
  - name: trojan-ws
    server: server
    port: 443
    type: trojan
    password: "example"
    network: ws
    sni: example.com
    # skip-cert-verify: true
    # fingerprint: xxxx
    udp: true
    # ws-opts:
    #   path: /path
    #   headers:
    #     Host: example.com
    #   v2ray-http-upgrade: false
```

# Trojan-xtls

```yaml
  - name: "trojan-xtls"
    type: trojan
    server: server
    port: 443
    password: yourpsk
    flow: "xtls-rprx-direct" # xtls-rprx-origin xtls-rprx-direct
    flow-show: true
    # udp: true
    # sni: example.com # aka server name
    # skip-cert-verify: true
    # fingerprint: xxxx
```
