---
description: 自动选择将按照 url 测试结果使用延迟最低的一个策略/代理节点
---

# 自动选择

## 配置示例

```yaml
proxy-groups:
  - name: "自动选择"
    type: url-test
    proxies:
      - ss
      - ss
      - vmess
    url: 'https://www.gstatic.com/generate_204'
    interval: 300
   #tolerance: 50
   #lazy: true
   #disable-udp: true
```

### url

对组内代理节点进行延迟测试的URL,**建议为https,部分代理提供商会对http进行劫持修改**

### **interval**

间隔多长时间进行一次测试,单位为秒

### tolerance

容差,新的最低延迟的代理延迟大于之前选择的节点延迟减去容差值时,才会切换节点,默认0,单位ms

例: 上次选择的代理节点这次测试延迟为50ms,容差10,则必须有低于40ms的节点才会切换

### name

该策略组的名字

### type

该策略组的类型,自动选择策略为 `url-test`

### lazy

打开lazy时,未选择到当前策略组时,则不会进行测试

### disable-udp

禁用该策略组的UDP
