# Обнаружение доменов

```{.yaml linenums="1"}
sniffer:
  enable: false
  force-dns-mapping: true
  parse-pure-ip: true
  override-destination: false
  sniff:
    HTTP:
      ports: [80, 8080-8880]
      override-destination: true
    TLS:
      ports: [443, 8443]
    QUIC:
      ports: [443, 8443]
  force-domain:
    - +.v2ex.com
  skip-domain:
    - Mijia Cloud
  skip-src-address:
    - 192.168.0.3/32
  skip-dst-address:
    - 192.168.0.3/32
```

## enable

Включить или отключить sniffer.

## force-dns-mapping

Принудительное обнаружение для трафика типа `redir-host`.

## parse-pure-ip

Принудительное обнаружение для всего трафика, для которого не удалось получить доменное имя.

## override-destination

Использовать ли результат обнаружения в качестве фактического адреса доступа, по умолчанию true.

## sniff

Настройки протоколов для обнаружения, поддерживаются только `HTTP`/`TLS`/`QUIC`.

- `ports`: [диапазоны портов](../../handbook/syntax.ru.md#диапазоны-портов)
- `override-destination`: переопределяет глобальную настройку `override-destination`

## force-domain

Список доменов для принудительного обнаружения, использует [подстановочные знаки доменов](../../handbook/syntax.ru.md#подстановочные-знаки-для-доменов).

## skip-domain

Список доменов для пропуска обнаружения, использует [подстановочные знаки доменов](../../handbook/syntax.ru.md#подстановочные-знаки-для-доменов).

## skip-src-address

Список диапазонов исходных IP-адресов для пропуска обнаружения.

## skip-dst-address

Список диапазонов целевых IP-адресов для пропуска обнаружения. 