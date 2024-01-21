---
description: 当代理节点url测试超时时,按照节点顺序选择
---

```{.yaml linenums="1"}
proxy-groups:
- name: "fallback"
  type: fallback
  proxies:
  - ss
  - vmess
  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  #lazy: true
```

## 通用字段

参阅 [通用字段](./index.md)

## url

对组内代理节点进行延迟健康检查的URL,**建议为https,部分代理提供商会对http进行劫持修改**

## interval

间隔多长时间进行一次健康检查,单位为秒

## lazy

打开lazy时,未选择到当前策略组时,则不会进行测试
