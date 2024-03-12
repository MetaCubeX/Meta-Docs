# Trojan

```{.yaml linenums="1"}
proxies:
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

[通用字段](./index.md)

## Trojan-grpc

```{.yaml linenums="1"}
proxies:
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

## Trojan-ws

```{.yaml linenums="1"}
proxies:
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

## Trojan-xtls

```{.yaml linenums="1"}
proxies:
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
