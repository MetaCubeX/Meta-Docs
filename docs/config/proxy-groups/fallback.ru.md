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

Если текущий узел не отвечает по таймауту, будет выбран первый доступный узел в порядке списка прокси

## Общие поля

См. [Общие поля](./index.md) 