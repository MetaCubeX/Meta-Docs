# dialer-proxy

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  dialer-proxy: dialer
  ...

- name: "ss2"
  ...

proxy-groups:
- name: dialer
  type: select
  proxies:
  - ss2
```

Указывает, что текущий `proxies` устанавливает сетевые соединения через `dialer-proxy`. Значение может быть `name` из [группы политик](../proxy-groups/index.md) или [исходящих прокси](../proxies/index.md)

В приведенном примере ss1 устанавливает соединение через ss2

При использовании ss1 в качестве прокси образуется цепочка `ядро ---ss1--> обертка ss2 ===ss2==> сервер ss2 ---ss1--> сервер ss1 --> цель` 