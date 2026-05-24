# Подправила (SUB-RULE)

Блок **sub-rules** — подсистема изолированных списков правил. Работают как «подпрограммы»: можно вынести специфическую логику из глобального `rules:` и вызывать по имени.

```{.yaml linenums="1"}
sub-rules:
  rule1:
    - DOMAIN-SUFFIX,baidu.com,DIRECT
    - MATCH,PROXY
  sub-rule2:
    - IP-CIDR,1.1.1.1/32,REJECT
    - IP-CIDR,8.8.8.8/32,ss1
    - DOMAIN,dns.alidns.com,REJECT
```

## Вызов из rules

Через селектор `SUB-RULE` в глобальном блоке `rules:`:

```{.yaml linenums="1"}
rules:
  - SUB-RULE,(NETWORK,tcp),sub-rule2   # если TCP — проверить в sub-rule2
  - GEOSITE,google,PROXY
  - MATCH,DIRECT
```

## Привязка к listener

Можно заставить конкретный входящий порт обрабатывать трафик по уникальным правилам, игнорируя глобальный блок `rules:`:

```{.yaml linenums="1"}
listeners:
  - name: guest-socks
    type: socks
    port: 10890
    rule: rule1   # трафик на этом порту обрабатывается только sub-rule1
```

!!! tip "Применение"
    Изолированные «гостевые» порты или порты для детского интернета с жёстким чёрным списком.

## Преимущество

Sub-rules снижают нагрузку на CPU: вместо прогона каждого пакета по огромному глобальному списку правил Mihomo отправляет пакет в короткий специализированный список по условию (протокол, порт и т.д.).
