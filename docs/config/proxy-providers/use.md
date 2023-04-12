---
description: 展示如何在策略组内引用代理集合内的代理节点
---

# 引用代理集

## 配置示例

```yaml
proxy-providers:
  meta:
    type: http
    path: ./meta.yaml
    url: http://example.com/files/meta.yaml
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300

proxy-groups: 
  - name: Proxy
    type: select
    use:
      - meta
```
