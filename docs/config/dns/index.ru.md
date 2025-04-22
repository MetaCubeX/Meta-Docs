# DNS

```{.yaml linenums="1"}
dns:
  enable: true
  cache-algorithm: arc
  prefer-h3: false
  use-hosts: true
  use-system-hosts: true
  respect-rules: false
  listen: 0.0.0.0:1053
  ipv6: false
  default-nameserver:
    - 223.5.5.5
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter-mode: blacklist
  fake-ip-filter:
    - '*.lan'
  nameserver-policy:
    '+.arpa': '10.0.0.1'
    'rule-set:cn':
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  proxy-server-nameserver:
    - https://doh.pub/dns-query
  direct-nameserver:
    - system
  direct-nameserver-follow-policy: false
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
```

## enable

Включает или отключает DNS. Если установлено значение false, используется системный DNS для разрешения.

## cache-algorithm

Поддерживаемые алгоритмы:

- lru: Least Recently Used (наименее недавно использованный), значение по умолчанию
- arc: Adaptive Replacement Cache (адаптивная замена кэша)

## prefer-h3

Предпочитать использование http/3 для DOH

## listen

Прослушивание DNS сервиса, поддерживает udp, tcp

## IPV6

Разрешать IPV6 или нет. Если установлено значение false, то для запросов AAAA будет возвращаться пустой ответ.

## enhanced-mode

Возможные значения: `fake-ip`/`redir-host`, по умолчанию `redir-host`

Режим обработки DNS в mihomo

## fake-ip-range

Настройка диапазона IP для режима fakeip. Адрес IPv4 по умолчанию для [tun](../inbound/tun.md) также использует это значение в качестве ориентира.

## fake-ip-filter

Фильтр fakeip. Для указанных адресов не будет назначено сопоставление fakeip для соединения.

Значения поддерживают [маску доменов](../../handbook/syntax.md#_8) и [импорт наборов доменов](../../handbook/syntax.md#_13)

## fake-ip-filter-mode: blacklist

Возможные режимы: `blacklist`/`whitelist`, по умолчанию `blacklist`. При выборе `whitelist` возвращается fake-ip только для доменов, соответствующих фильтру.

## use-hosts

Использовать [hosts](./hosts.md) из конфигурации или нет, по умолчанию true

## use-system-hosts

Использовать системные hosts или нет, по умолчанию true

## respect-rules

DNS-соединения следуют [правилам маршрутизации](../rules/index.md), требуется настройка [proxy-server-nameserver](./index.md#proxy-server-nameserver)

!!! note ""
    Настоятельно не рекомендуется использовать вместе с `prefer-h3`

## default-nameserver

DNS по умолчанию, используется для разрешения доменных имен DNS-серверов

!!! note ""
    Должен быть указан IP, может быть зашифрованным DNS

## nameserver-policy

Указывает серверы разрешения имен для конкретных доменов, может использовать geosite, имеет приоритет над запросами `nameserver/fallback`

Ключи поддерживают [маску доменов](../../handbook/syntax.md#_8)

Значения поддерживают строки/массивы

## proxy-server-nameserver

Сервер разрешения имен для доменов прокси-узлов, используется только для разрешения доменных имен прокси-узлов. Если не указан, следует конфигурации nameserver-policy, nameserver и fallback.

## direct-nameserver

DNS-сервер для разрешения доменных имен через DIRECT выход. Если не указан, следует конфигурации nameserver-policy, nameserver и fallback.

## direct-nameserver-follow-policy

Следовать ли nameserver-policy, по умолчанию нет. Работает только если direct-nameserver не пустой.

## nameserver

Серверы разрешения доменных имен по умолчанию

## fallback

Резервные серверы разрешения доменных имен, обычно используются зарубежные DNS-серверы для обеспечения достоверности результатов.

После настройки `fallback` по умолчанию включается `fallback-filter` с `geoip-code` равным cn.

## fallback-filter

Фильтр резервных серверов разрешения доменных имен. Если условия соответствуют, будет использован результат `fallback` или для разрешения будет использован только `fallback`.

### geoip

Включить или отключить определение geoip

### geoip-code

Возможные значения - сокращения названий стран, по умолчанию `CN`

IP-результаты, не принадлежащие стране, настроенной в `geoip-code`, считаются загрязненными.

Результаты для страны, настроенной в `geoip-code`, будут приняты напрямую, иначе будет принят результат `fallback`.

### geosite

Возможные значения - соответствующие наборы в geosite.

Содержимое списка geosite считается загрязненным. Для доменов, соответствующих geosite, будет использоваться только разрешение через `fallback`, без использования `nameserver`.

### ipcidr

Содержимое записывается в формате `IP/маска`

Результаты для этих сетей считаются загрязненными. Когда `nameserver` возвращает эти результаты, будет использован результат разрешения через `fallback`.

### domain

Эти домены считаются загрязненными. При совпадении с этими доменами будет напрямую использоваться разрешение через `fallback`, без использования `nameserver`.

## Дополнительные параметры

Эта часть может использоваться для DNS-серверов в публичной сети, добавляется с помощью `#`, различные параметры соединяются с помощью `&`.

Поддерживаемый диапазон:

|              | [nameserver](./index.md#nameserver) | [fallback](./index.md#fallback) | [nameserver-policy](./index.md#nameserver-policy) | [proxy-server-nameserver](./index.md#proxy-server-nameserver) | [direct-nameserver](./index.md#direct-nameserver) | [default-nameserver](./index.md#default-nameserver) | [WireGuard.dns](../proxies/wg.md#dns) |
|--------------|---------------------------|-------------------|---------------------|-------------------------|---------------------|----------------------|------------------------------------|
| Указание прокси/интерфейса | ✓                         | ✓                 | ✓                   | ✓                       | ✓                   | ✓                    | ✓                                  |
| Принудительный HTTP/3 | DOH                       | DOH               | DOH                 | DOH                     | DOH                 | DOH                  | DOH                                |
| Пропуск проверки сертификата | DOH                       | DOH               | DOH                 | DOH                     | DOH                 | DOH                  | DOH                                |
| ecs          | DOH                       | DOH               | DOH                 | DOH                     | DOH                 | DOH                  | DOH                                |
| ecs-override | DOH                       | DOH               | DOH                 | DOH                     | DOH                 | DOH                  | DOH                                |

> В таблице выше `✓` означает поддержку [всех типов](./type.md)

### DNS с указанием прокси/интерфейса для соединения

Приоритет отдается существующим прокси. Если прокси с указанным именем не существует, используется указанный интерфейс.

`#RULES` используется для следования правилам маршрутизации, аналогично [respect-rules](./index.md#respect-rules)

Для запросов через прокси следует настроить `proxy-server-nameserver`, чтобы избежать проблемы курицы и яйца.

```{.yaml linenums="1"}
nameserver:
  - 'tls://dns.google#proxy'
  - 'tls://dns.alidns.com#eth0'
```

### Принудительный HTTP/3

Эта опция не конфликтует с `prefer-h3`. При включении принудительно использует HTTP/3 для соединений DOH. Перед использованием убедитесь, что DOH-сервер поддерживает HTTP/3.

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#h3=true'
```

### Пропуск проверки сертификата doh

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#skip-cert-verify=true'
```

### ecs

Указывает адрес subnet для DNS-запроса, поддерживается только [doh](./type.md#dns-over-https)

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24'
```

### ecs-override

Принудительная замена адреса subnet для DNS-запроса, поддерживается только [doh](./type.md#dns-over-https)

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24&ecs-override=true'
``` 