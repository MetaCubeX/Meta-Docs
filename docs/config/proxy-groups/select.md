---
description: 手动选择一个策略/代理节点
---

# 手动选择

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

### name

该策略组的名字,如有特殊符号,应当使用引号将其包裹

```
- name: "Global Proxy"
```

### type

该策略组的类型,手动选择策略为 `select`

### proxies

该策略组包含的代理

### disable-udp

禁用该策略组的UDP
