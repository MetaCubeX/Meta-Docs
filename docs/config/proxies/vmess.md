# VMess

```{.yaml linenums="1"}
proxies:
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
    # path: /path
    # headers:
    #   Host: v2ray.com
    # max-early-data: 2048
    # early-data-header-name: Sec-WebSocket-Protocol
    # v2ray-http-upgrade: false
```

[通用字段](./index.md)

## VMess-h2

```{.yaml linenums="1"}
proxies:
- name: "vmess-h2"
  type: vmess
  server: server
  port: 443
  uuid: uuid
  alterId: 32
  cipher: auto
  network: h2
  tls: true
  # fingerprint: xxxx
  h2-opts:
    host:
      - http.example.com
      - http-alt.example.com
    path: /
```

## VMess-http

```{.yaml linenums="1"}
proxies:
- name: "vmess-http"
  type: vmess
  server: server
  port: 443
  uuid: uuid
  alterId: 32
  cipher: auto
  # udp: true
  # network: http
  # http-opts:
  #   method: "GET"
  #   path:
  #     - '/'
  #     - '/video'
  #   headers:
  #     Connection:
  #       - keep-alive
  # ip-version: ipv4 # 设置使用 IP 类型偏好，可选：ipv4，ipv6，dual，默认值：dual
```

## VMess-gRPC

```{.yaml linenums="1"}
proxies:
- name: vmess-grpc
  server: server
  port: 443
  type: vmess
  uuid: uuid
  alterId: 32
  cipher: auto
  network: grpc
  tls: true
  # fingerprint: xxxx
  servername: example.com
  # skip-cert-verify: true
  grpc-opts:
    grpc-service-name: "example"
  # ip-version: ipv4
```
