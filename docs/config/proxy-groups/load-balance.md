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
  #strategy: consistent-hashing
```

## 通用字段

参阅 [通用字段](./index.md)

## strategy

负载均衡策略

* `round-robin` 将会把所有的请求分配给策略组内不同的代理节点

* `consistent-hashing` 将相同的 `目标地址` 的请求分配给策略组内的同一个代理节点

* `sticky-sessions`: 将相同的 `来源地址` 和 `目标地址` 的请求分配给策略组内的同一个代理节点，缓存 10 分钟过期

!!! note
    `目标地址` 为域名时，使用顶级域名匹配