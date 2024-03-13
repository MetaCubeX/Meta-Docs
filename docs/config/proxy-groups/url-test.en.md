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

## Common Fields

Refer to [Common Fields](./index.md).

## tolerance

proxies switch tolerance, measured in milliseconds (ms).
