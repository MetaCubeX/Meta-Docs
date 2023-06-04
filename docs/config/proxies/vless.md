# VLESS

!!! note
    Clash 的 H2 传输层未实现多路复用功能，在 Clash.Meta 中更建议使用 gRPC 协议

Meta 增加了 VLESS 协议支持，具体格式如下：

#### VLESS-xtls-rprx-vision

```yaml
- name: "vless-vision"
  type: vless
  server: server
  port: 443
  uuid: uuid
  network: tcp
  tls: true
  udp: true
  flow: xtls-rprx-vision 
  client-fingerprint: chrome
  # xudp: true #default
  # fingerprint: xxxx
  # skip-cert-verify: true
```

!!! note
    Meta 的 `xtls-*` 流控实际上与 Xray-core 中的 `xtls-*-udp443` 等效，如需拦截 443 端口的 UDP 流量，请使用逻辑规则：

    `AND,((NETWORK,UDP),(DST-PORT,443)),REJECT`

#### VLESS-reality-vision

```yaml
- name: "vless-reality-vision"
  type: vless
  server: server
  port: 443
  uuid: uuid
  network: tcp
  tls: true
  udp: true
  flow: xtls-rprx-vision
  servername: speed.cloudflare.com # REALITY servername
  reality-opts:
    public-key: xxx
    short-id: xxx # optional
  client-fingerprint: chrome # cannot be empty
```

#### VLESS-reality-grpc

```yaml
- name: "vless-reality-grpc"
  type: vless
  server: server
  port: 443
  uuid: uuid
  network: grpc
  tls: true
  udp: true
  flow:
  client-fingerprint: chrome
  servername: testingcf.jsdelivr.net # REALITY servername
  grpc-opts:
    grpc-service-name: "grpc"
  reality-opts:
    public-key: CrrQSjAG_YkHLwvM2M-7XkKJilgL5upBKCp0od0tLhE
    short-id: 10f897e26c4b9478
```

#### VLESS-TCP-TLS

```yaml
- name: "vless-tcp"
  type: vless
  server: server
  port: 443
  uuid: uuid
  network: tcp
  tls: true
  servername: example.com # AKA SNI
  # flow: xtls-rprx-direct # xtls-rprx-origin  # enable XTLS
  # skip-cert-verify: true

```

#### VLESS-WS-TLS

```yaml
- name: "vless-ws"
  type: vless
  server: server
  port: 443
  uuid: uuid
  udp: true
  tls: true
  network: ws
  servername: example.com # priority over wss host
  # skip-cert-verify: true
  ws-opts:
    path: "/"
    headers:
      Host: example.com
```
