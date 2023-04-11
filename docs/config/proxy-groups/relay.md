---
description: 代理链,若落地协议支持 UDP over TCP 则可支持 UDP
---

# 链式代理

## 配置示例

```yaml
Proxy Groups:
- name: "relay"
  type: relay
  proxies:
    - vmess
    - ss1
    - ss2
 #disable-udp: true
```

流量去向为 clash > vmess > ss1 > ss2 > Internet
