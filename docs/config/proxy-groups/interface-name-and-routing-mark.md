---
description: 可以为策略组指定出站接口以及出站流量标记
---

# 指定接口以及路由标记

## 配置示例

```yaml
proxy-groups:
    - name: "直连"
    type: select
    proxies:
      - "en0直连"
      - "en1直连"
  - name: "en1直连"
    type: select
    interface-name: en1
    routing-mark: 19198
    proxies:
      - DIRECT
  - name: "en0直连"
    type: select
    interface-name: en0
    routing-mark: 11451
    proxies:
      - DIRECT
```

优先级: 代理节点 > 代理策略 > 全局
