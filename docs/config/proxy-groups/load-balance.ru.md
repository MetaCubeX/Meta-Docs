# Load-balance

```{.yaml linenums="1"}
proxy-groups:
- name: "load-balance"
  type: load-balance
  proxies:
  - ss1
  - ss2
  - vmess1
  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  #lazy: true
  #strategy: consistent-hashing # или round-robin
```

## Общие поля

См. [Общие поля](./index.md)

## strategy

Стратегии балансировки нагрузки

* `round-robin` распределяет все запросы между разными прокси-узлами в группе стратегий.

* `consistent-hashing` назначает запросы с одинаковым `целевым адресом` одному и тому же прокси-узлу в группе стратегий.

* `sticky-sessions`: запросы с одинаковым `исходным адресом` и `целевым адресом` будут направляться к одному и тому же прокси-узлу в группе стратегий, срок действия кэша - 10 минут.

!!! note
    Когда `целевой адрес` является доменом, используется сопоставление по домену верхнего уровня. 