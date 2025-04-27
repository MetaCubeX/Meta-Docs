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

## Общие поля

См. [Общие поля](./index.md)

## tolerance

Допустимая погрешность при переключении узлов, измеряется в миллисекундах (мс). 