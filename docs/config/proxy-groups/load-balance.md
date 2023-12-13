---
description: 负载均衡将按照算法随机选择节点
---
## 配置示例

```yaml
proxy-groups:
  - name: "load-balance"
    type: load-balance
    proxies:
      - ss1
      - ss2
      - vmess1
    url: 'https://www.gstatic.com/generate_204'
    interval: 300
   #lazy: true
   #disable-udp: true
   #strategy: consistent-hashing # or round-robin
```

### url

对组内代理节点进行延迟测试的URL,**建议为https,部分代理提供商会对http进行劫持修改**

### **interval**

间隔多长时间进行一次测试,单位为秒

### strategy

负载均衡策略

`consistent-hashing` 将会把相同顶级域名的请求分配给策略组内的同一个代理节点

`round-robin` 将会把所有的请求分配给策略组内不同的代理节点

### name

该策略组的名字

### type

该策略组的类型

### lazy

打开lazy时,未选择到当前策略组时,则不会进行测试

### disable-udp

禁用该策略组的UDP
