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

If the current node times out, the first available node will be selected in the order of proxies

## Common Fields

Refer to [Common Fields](./index.md).
