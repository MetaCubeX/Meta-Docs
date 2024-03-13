# Load-balance

```{.yaml linenums="1"}
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
  #strategy: consistent-hashing # or round-robin
```

## 通用字段

参阅 [通用字段](./index.md)

## strategy

负载均衡策略

* `consistent-hashing` 将会把相同顶级域名的请求分配给策略组内的同一个代理节点

* `round-robin` 将会把所有的请求分配给策略组内不同的代理节点
