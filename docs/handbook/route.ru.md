# Маршрутизация

Mihomo направляет трафик на основе правил из блока `rules:`.

## Принцип работы

1. Входящий пакет (TCP/UDP) попадает в Mihomo через один из портов или TUN
2. Mihomo по возможности определяет домен (через DNS + sniffer)
3. Пакет последовательно сравнивается с каждым правилом в блоке `rules:` сверху вниз
4. При первом совпадении трафик направляется на указанную политику (прокси, DIRECT, REJECT)

## Типы правил

- **Доменные**: `DOMAIN`, `DOMAIN-SUFFIX`, `DOMAIN-KEYWORD`, `DOMAIN-WILDCARD`, `DOMAIN-REGEX`, `GEOSITE`
- **IP-адреса**: `IP-CIDR`, `IP-CIDR6`, `IP-SUFFIX`, `IP-ASN`, `GEOIP`
- **Источник**: `SRC-GEOIP`, `SRC-IP-ASN`, `SRC-IP-CIDR`, `SRC-IP-SUFFIX`
- **Порты**: `DST-PORT`, `SRC-PORT`
- **Сеть**: `NETWORK` (TCP/UDP), `DSCP`
- **Входящие метаданные**: `IN-PORT`, `IN-TYPE`, `IN-USER`, `IN-NAME`
- **Процессы**: `PROCESS-PATH`, `PROCESS-NAME`, `UID`
- **Логические**: `AND`, `OR`, `NOT`
- **Ссылочные**: `RULE-SET`, `SUB-RULE`
- **Финальное**: `MATCH` — ловит весь трафик, не подошедший ни под одно правило выше

## Порядок имеет значение

Первое совпавшее правило выигрывает — весь трафик, подходящий под него, уходит на указанную политику, остальные правила игнорируются.

```{.yaml linenums="1"}
rules:
  - DOMAIN,ad.com,REJECT         # реклама — в REJECT
  - GEOSITE,youtube,DIRECT       # ютуб — напрямую
  - GEOIP,RU,DIRECT              # российские IP — напрямую
  - MATCH,PROXY                  # всё остальное — через прокси
```

Подробнее: [Правила маршрутизации](../config/rules/index.md).
