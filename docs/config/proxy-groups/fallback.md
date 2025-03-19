# Fallback

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

当前节点超时时，则会按代理顺序选择第一个可用节点

## 通用字段

参阅 [通用字段](./index.md)
