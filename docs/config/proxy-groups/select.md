---
description: 手动选择一个策略/代理节点
---
## 配置示例

```yaml
proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - ss
      - ss
      - vmess
      - auto
   #disable-udp: true
```

## 通用字段
参阅 [通用字段](./index.md)
