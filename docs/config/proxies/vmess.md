# VMess

```yaml
  - name: "vmess"
    type: vmess
    server: server
    port: 443
    uuid: uuid
    alterId: 32
    cipher: auto
    # udp: true
    # tls: true
    # fingerprint: xxxx
    # client-fingerprint: chrome    # Available: "chrome","firefox","safari","ios","random", currently only support TLS transport in TCP/GRPC/WS/HTTP for VLESS/Vmess and trojan.
    # skip-cert-verify: true
    # servername: example.com # priority over wss host
    # network: ws
    # ws-opts:
    #   path: /path
    #   headers:
    #     Host: v2ray.com
    #   max-early-data: 2048
    #   early-data-header-name: Sec-WebSocket-Protocol
```
