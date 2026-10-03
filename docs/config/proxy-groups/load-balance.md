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
  #hash-key: in-user
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

## hash-key

哈希 key，可选值 `in-user`，仅 `consistent-hashing` 和 `sticky-sessions` 支持；`round-robin` 不做哈希，配置后会报错而不是被忽略

* `in-user` 使用入站认证用户名作为哈希 key（与 `IN-USER` 规则读取的是同一字段），它不随目标地址变化，因此一件跨多个域名的任务不会中途更换节点、更换出口 IP；未认证的请求回退到该策略原本的 key

* 不填时行为与旧版完全一致