# TPROXY

```{.yaml linenums="1"}
listeners:
- name: tproxy-in
  type: tproxy
  port: 7894
  listen: 0.0.0.0
  udp: true
```

## [Общие поля](./index.md)

## Настройка протокола

### udp

Прослушивать ли UDP 