---
description: 当代理节点url测试超时时,按照节点顺序选择
---

# 自动回退

## 配置示例

```yaml
proxy-groups:
  - name: "fallback"
    type: fallback
    proxies:
      - ss
      - ss
      - vmess
    url: 'https://www.gstatic.com/generate_204'
    interval: 300
   #lazy: true
   #disable-udp: true
```

### url

对组内代理节点进行延迟测试的URL,**建议为https,部分代理提供商会对http进行劫持修改**

### **interval**

间隔多长时间进行一次测试,单位为秒

### name

该策略组的名字

### type

该策略组的类型,自动回退策略为 `fallback`

### lazy

打开lazy时,未选择到当前策略组时,则不会进行测试

### disable-udp

禁用该策略组的UDP
