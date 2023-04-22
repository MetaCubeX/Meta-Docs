---
description: 代理链,若落地协议支持 UDP over TCP 则可支持 UDP
---

# 链式代理

## 配置示例

```yaml
Proxy Groups:
# 代理链，目前 relay 可以支持 udp 的只有 vmess/vless/trojan/ss/ssr/tuic
# wireguard目前不支持在relay中使用，请使用 proxy 中的 dialer-proxy 配置项
# Traffic: clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet
- name: "relay"
  type: relay
  proxies:
    - http
    - vmess
    - ss1
    - ss2
```

流量去向为 clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet
