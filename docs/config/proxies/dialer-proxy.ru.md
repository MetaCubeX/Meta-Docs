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

## Миграция relay

Тип proxy-group `relay` будет устаревшим, а proxy-group напрямую не поддерживает dialer-proxy. Поэтому для некоторых сценариев использования приведён следующий пример перехода.

### Вариант: relay с несколькими select

Исходная конфигурация:

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
  - { name: "proxy3", type: "socks" }
  - { name: "proxy4", type: "socks" }
proxy-groups:
  - { name: "relay-proxy", type: relay, proxies: ["select1", "select2"] }
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, proxies: ["proxy3", "proxy4"] }
rules:
  - MATCH,relay-proxy
```

При миграции на схему с dialer-proxy необходимо определить proxy3 и proxy4 через proxy-provider, а также с помощью override задать для всех прокси этого провайдера dialer-proxy, как показано ниже:

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
proxy-groups:
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, use: ["provider1"] }
proxy-providers:
  provider1:
    type: inline
    override:
      dialer-proxy: select1
    payload:
      - { name: "proxy3", type: "socks" }
      - { name: "proxy4", type: "socks" }
rules:
  - MATCH,select2
```