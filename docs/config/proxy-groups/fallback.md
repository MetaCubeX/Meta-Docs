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

当前选择节点超时，则会按顺序切换到下一个可以节点

## 通用字段

参阅 [通用字段](./index.md)
