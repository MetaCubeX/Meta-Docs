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

流量去向为 Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet

> wireguard目前不支持在relay中使用。如果需要，请使用 proxy 中的 dialer-proxy 配置项

### 关于UDP

relay支持传输UDP，前提是代理链的头尾节点都要支持 UDP over TCP。目前支持 udp 的协议有 `vmess`/`vless`/`trojan`/`ss`/`ssr`/`tuic`
