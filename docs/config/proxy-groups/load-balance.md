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
  # hash-key: in-user
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

哈希策略取 key 的方式，仅在 `strategy` 为 `consistent-hashing` 或 `sticky-sessions` 时有效

* `in-user` 使用入站认证的用户名作为 key（与 [`IN-USER`](../rules/index.md#in-user) 规则读取的是同一个字段）。它不随目标地址变化，因此一件跨多个域名的任务不会中途换节点、换出口 IP；未认证的请求回退到该策略原本的 key
* 不填时行为与旧版完全一致

!!! warning ""
    `round-robin` 不做哈希，配置 `hash-key` 会报错而不是被忽略

!!! note ""
    该选项只改变「拿什么值去哈希」，不改变策略拿到哈希值之后如何选节点。两种策略的一致性时长也不同：`consistent-hashing` 是永久的，而 `sticky-sessions` 的缓存 10 分钟过期