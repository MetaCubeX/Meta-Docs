# Сниффинг доменов

Блок **sniffer** перехватывает трафик на сетевом уровне и «заглядывает» внутрь пакетов (без расшифровки контента), чтобы прочитать SNI/hostname. Это критически важно в режиме `fake-ip` или когда приложения обращаются к серверам по IP без DNS-запроса.

```{.yaml linenums="1"}
sniffer:
  enable: true
  force-dns-mapping: true
  parse-pure-ip: true
  override-destination: true
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

Включение или отключение сниффера.

## force-dns-mapping

Принудительное сопоставление доменов для режима `redir-host`.

## parse-pure-ip

Сниффер анализирует любой «чистый» IP-трафик, для которого не было обнаружено доменное имя.

## override-destination

Перезаписывать оригинальный IP-адрес назначения тем адресом, который сниффер получил после определения домена. Рекомендуется `true`.

## sniff

Настройки протоколов для сниффинга. Поддерживаются только `HTTP`, `TLS`, `QUIC`.

- `ports`: [диапазоны портов](../../handbook/syntax.md#port-ranges)
- `override-destination`: переопределяет глобальную настройку `override-destination` для конкретного протокола

## force-domain

Список доменов для принудительного сниффинга, поддерживает [подстановочные знаки доменов](../../handbook/syntax.md#domain-wildcards).

## skip-domain

Список доменов, которые сниффер **не должен** обрабатывать.

!!! tip "Совместная работа с nfqws2 — пример"
    Если вы используете nfqws2 (zapret), стоит занести в `skip-domain` зоны, где подмена адреса после сниффинга нежелательна. Иначе sniffer может расходиться с тем, что уже «исправил» nfqws:
    ```yaml
    sniffer:
      sniff:
        TLS:
          ports: [443, 8443]
          skip-domain:
            - 'geosite:google'
            - 'geosite:discord'
            - 'geosite:youtube'
            - 'geosite:ru'
        HTTP:
          ports: [80, 8080-8880]
          skip-domain:
            - 'geosite:google'
            - 'geosite:discord'
            - 'geosite:ru'
        QUIC:
          ports: [443, 5055, 5056]
          skip-domain:
            - 'geosite:google'
            - 'geosite:discord'
            - 'geosite:ru'
    ```

## skip-src-address

Список диапазонов исходных IP-адресов, для которых сниффер отключён. Полезно, чтобы запретить сниффинг для конкретного устройства (ТВ, приставка).

## skip-dst-address

Список диапазонов целевых IP-адресов, которые сниффер игнорирует (например, DNS-сервера или локальные адреса провайдера).
