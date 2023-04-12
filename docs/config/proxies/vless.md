# vless

!!! note
    因clash实现的问题,h2传输层不建议在clash使用,建议更换为grpc
    

Meta增加了Vless协议支持,具体格式如下：

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

#### **VLESS-reality-vision**


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
  servername: www.microsoft.com # REALITY servername
  reality-opts:
    public-key: xxx
    short-id: xxx # optional
  client-fingerprint: chrome # cannot be empty
```

#### **VLESS-reality-grpc**

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
  # skip-cert-verify: true
  client-fingerprint: chrome
  servername: testingcf.jsdelivr.net
  grpc-opts:
    grpc-service-name: "grpc"
  reality-opts:
    public-key: CrrQSjAG_YkHLwvM2M-7XkKJilgL5upBKCp0od0tLhE
    short-id: 10f897e26c4b9478
```

#### VLESS-TCP

```yaml
- name: "vless-tcp"
  type: vless
  server: server
  port: 443
  uuid: uuid
  network: tcp
  servername: example.com # AKA SNI
  # flow: xtls-rprx-direct # xtls-rprx-origin  # enable XTLS
  # skip-cert-verify: true

```

#### VLESS-WS



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
```yaml
