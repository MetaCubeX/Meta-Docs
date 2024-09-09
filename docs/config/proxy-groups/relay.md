# Relay

```{.yaml linenums="1"}
Proxy Groups:
# Traffic: Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet
- name: "relay"
  type: relay
  proxies:
    - http
    - vmess
    - ss1
    - ss2
```

!!! warning
    relay 策略即将被弃用，请使用[dialer-proxy](../proxies/index.md#dialer-proxy)

    wireguard 目前不支持在 relay 中使用，也请使用[dialer-proxy](../proxies/index.md#dialer-proxy)


流量去向为 Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet

## 关于 UDP

relay 支持传输 UDP，前提是代理链的头尾节点都要支持 UDP over TCP。目前支持 udp 的协议有 `vmess`/`vless`/`trojan`/`ss`/`ssr`/`tuic`
