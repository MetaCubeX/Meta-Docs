# Url-test

```{.yaml linenums="1"}
proxy-groups:
- name: "自动选择"
  type: url-test
  proxies:
  - ss
  - vmess
  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  #tolerance: 50
  #lazy: true
```

## 通用字段

参阅 [通用字段](./index.md)

## tolerance

节点切换容差，单位 ms
