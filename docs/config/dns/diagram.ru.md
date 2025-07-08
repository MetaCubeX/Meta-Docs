# Процесс разрешения

## Пример конфигурации

```{.yaml linenums="1"}
dns:
  nameserver:
    - https://doh.pub/dns-query
  fallback:
    - https://8.8.8.8/dns-query
  direct-nameserver:
    - system
  nameserver-policy:
    "geosite:cn,private":
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'

rules:
- DOMAIN-SUFFIX,google.com,PROXY
- GEOIP,CN,DIRECT
- MATCH,PROXY
```

## Процесс

!!! note ""
    Эта часть описывает только процесс обработки в модуле dns

!!! warning ""
    ~~Повторное разрешение через direct-nameserver раньше работало только для TCP-соединений~~  
    Начиная с v1.19.10, повторное разрешение через direct-nameserver также применяется к UDP-соединениям (для входящих Tun только в режиме Fakeip)

```mermaid
flowchart TD
  Rule[Сопоставление правил]
  Domain[Сопоставление с правилами для доменов]
  IP[Сопоставление с правилами для целевых IP]
  Resolve[Разрешение домена]

  NameServer[Использование nameserver для запроса]
  Policy[Сопоставление с nameserver-policy]
  Concurrent[Параллельный запрос с nameserver и fallback]
  Filter[Сопоставление с fallback-filter]
  DirectNS[Повторное разрешение с direct-nameserver]

  GetIP[Получение IP из запроса]

  Proxy[Отправка домена прокси]
  Direct[Прямое подключение с использованием IP]

  Rule -->  Domain
  Rule --> IP

  Domain -- Домен соответствует правилу DIRECT --> Resolve
  Domain -- Домен соответствует правилу DIRECT и настроен direct-nameserver --> DirectNS

  IP --> Resolve

  Resolve -- Настроен nameserver-policy --> Policy
  Policy -- Нет соответствия --> NameServer
  Policy --> GetIP
  Resolve -- Не настроен nameserver-policy --> NameServer

  NameServer -- Настроен fallback --> Concurrent
  Concurrent --> Filter
  Filter --> GetIP
  NameServer -- Не настроен fallback --> GetIP

  GetIP -- Соответствует DIRECT и настроен direct-nameserver --> DirectNS
  DirectNS --> Direct

  GetIP -- IP соответствует правилу для прокси --> Proxy[Отправка домена прокси]
  Domain -- Домен соответствует правилу для прокси --> Proxy

  GetIP -- IP соответствует правилу DIRECT --> Direct
``` 