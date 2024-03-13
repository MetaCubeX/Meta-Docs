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

If the currently selected node times out, the system will switch to the next available node in sequence.

## Common Fields

Refer to [Common Fields](./index.md).
