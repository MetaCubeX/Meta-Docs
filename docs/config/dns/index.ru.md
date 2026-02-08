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
  # fake-ip-range6: fdfe:dcba:9876::1/64
  fake-ip-filter-mode: blacklist
  fake-ip-filter:
    - '*.lan'
  # fake-ip-ttl: 1
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
  proxy-server-nameserver-policy:
    'www.yournode.com': '114.114.114.114'
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

## fake-ip-range6

Настройка диапазона IPv6 для режима fakeip.

## fake-ip-filter

Фильтр fakeip. Для указанных адресов не будет назначено сопоставление fakeip для соединения.

Значения поддерживают [маску доменов](../../handbook/syntax.md#_8) и [импорт наборов доменов](../../handbook/syntax.md#_13)

## fake-ip-filter-mode

Возможные режимы: `blacklist`/`whitelist`/`rule`, по умолчанию `blacklist`. При выборе `whitelist` возвращается fake-ip только для доменов, соответствующих фильтру.

Когда параметр `fake-ip-filter-mode` установлен в значение `rule`, включается режим на основе правил, и синтаксис `fake-ip-filter` изменяется:

```{.yaml linenums="1"}
dns:
  fake-ip-filter-mode: rule
  fake-ip-filter: # Логика сопоставления фиктивных IP-адресов соответствует правилам маршрутизации (сверху вниз), а синтаксис также согласован и поддерживает GEOSITE, RuleSet, DOMAIN* и MATCH.
    - RULE-SET,reject-domain,fake-ip # Для пользовательского поведения набора правил необходимо установить параметр domain/classical; если установить параметр classical, будут действовать только правила уровня домена.
    - RULE-SET,proxy-domain,fake-ip
    - GEOSITE,gfw,fake-ip
    - DOMAIN,www.baidu.com,real-ip
    - DOMAIN-SUFFIX,qq.com,real-ip
    - DOMAIN-SUFFIX,jd.com,fake-ip
    - MATCH,fake-ip # fake-ip or real-ip
```

## fake-ip-ttl

Настройте TTL, возвращаемый запросами fakeip; не изменяйте его без крайней необходимости.

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

## proxy-server-nameserver-policy

Формат тот же, что и у nameserver-policy, и используется только для разрешения доменных имен узлов. Он вступает в силу только в том случае, если proxy-server-nameserver не пуст.

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

Кроме указания прокси/интерфейса и ecs, значения остальных пунктов являются bool (true/false)

### Пример

```{.yaml linenums="1"}
dns:
  nameserver:
  - 'https://8.8.8.8/dns-query#proxy&ecs=1.1.1.1/24&ecs-override=true'
proxies:
- name: proxy
  type: ss
```

### DNS с указанием прокси/интерфейса для соединения

Приоритет отдается существующим прокси. Если прокси с указанным именем не существует, используется указанный интерфейс.

`#RULES` используется для следования правилам маршрутизации, аналогично [respect-rules](./index.md#respect-rules)

Для запросов через прокси следует настроить proxy-server-nameserver, чтобы избежать проблемы курицы и яйца.

### h3

Принудительный HTTP/3

Этот параметр не конфликтует с `prefer-h3`. После его заполнения принудительно включается HTTP/3 для установления DOH-соединения. Перед использованием убедитесь, что DOH-сервер поддерживает HTTP/3.

### skip-cert-verify

Пропустить проверку сертификата TLS

### ecs

Указать адрес подсети для DNS-запроса

### ecs-override

Принудительное переопределение адреса подсети DNS-запроса

### disable-ipv4

Отбрасывать A-ответы

### disable-ipv6

Отбрасывать AAAA-ответы

### disable-qtype-\<int\>

Например, отбрасывание определенных типов ответов, таких как `disable-qtype-65`, может блокировать разрешение DNS для типов HTTPS (TYPE65).
