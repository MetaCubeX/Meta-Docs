# Глобальная конфигурация

## Разрешить доступ из локальной сети

Разрешает другим устройствам использовать [порты прокси](./inbound/port.md) Clash для доступа в интернет

Возможные значения: `true/false`

```{.yaml linenums="1"}
allow-lan: true
```

Привязка к адресу, разрешает другим устройствам доступ только через этот адрес

***`"*"`*** привязка ко всем IP-адресам

***`192.168.31.31`*** привязка к одному IPv4-адресу

***`[aaaa::a8aa:ff:fe09:57d8]`*** привязка к одному IPv6-адресу

```{.yaml linenums="1"}
bind-address: "*"
```

Разрешенные диапазоны IP-адресов, работает только если `allow-lan` установлен в `true`
Значение по умолчанию: `0.0.0.0/0` и `::/0`

```{.yaml linenums="1"}
lan-allowed-ips:
- 0.0.0.0/0
- ::/0
```

Запрещенные диапазоны IP-адресов, черный список имеет приоритет над белым списком, по умолчанию пусто

```{.yaml linenums="1"}
lan-disallowed-ips:
- 192.168.0.3/32
```

### Аутентификация пользователей

Аутентификация пользователей для прокси `http(s)`/`socks`/`mixed`

```{.yaml linenums="1"}
authentication:
- "user1:pass1"
- "user2:pass2"
```

Установка диапазонов IP-адресов, которым разрешено пропускать аутентификацию

```{.yaml linenums="1"}
skip-auth-prefixes:
- 127.0.0.1/8
- ::1/128
```

## Режим работы

* ***`rule`*** режим правил
* ***`global`*** глобальный прокси (требуется выбрать прокси/политику в группе GLOBAL)
* ***`direct`*** глобальное прямое соединение

Этот параметр имеет значение по умолчанию - режим правил

```{.yaml linenums="1"}
mode: rule
```

## Уровень логирования

Уровень логирования ядра Clash, отображается только в консоли и на странице управления

```{.yaml linenums="1"}
log-level: info
```

* ***`silent`*** без вывода
* ***`error`*** вывод только ошибок, приводящих к невозможности использования
* ***`warning`*** вывод ошибок, не влияющих на работу, а также содержимое уровня error
* ***`info`*** вывод обычной информации о работе, а также содержимое уровней error и warning
* ***`debug`*** максимально подробный вывод всей информации о работе

## IPv6

Разрешить ядру принимать трафик IPv6

Возможные значения: `true/false`, по умолчанию `true`

```{.yaml linenums="1"}
ipv6: true
```

## Настройки TCP Keep Alive

Изменение этого параметра для уменьшения [проблем с энергопотреблением](https://github.com/vernesong/OpenClash/issues/2614) на мобильных устройствах

Интервал отправки пакетов TCP Keep Alive в секундах

```{.yaml linenums="1"}
keep-alive-interval: 15
```

Максимальное время бездействия TCP Keep Alive

```{.yaml linenums="1"}
keep-alive-idle: 15
```

Отключение TCP Keep Alive, на Android по умолчанию true

```{.yaml linenums="1"}
disable-keep-alive: false
```

## Режим сопоставления процессов

Управляет сопоставлением процессов в Clash

* ***`always`*** включено, принудительное сопоставление всех процессов
* ***`strict`*** по умолчанию, Clash сам определяет, включать ли сопоставление
* ***`off`*** без сопоставления процессов, рекомендуется использовать на роутерах

```{.yaml linenums="1"}
find-process-mode: strict
```

## Внешнее управление (API)

Внешний контроллер, позволяет управлять ядром Clash через RESTful API

API прослушивающий адрес, можно изменить 127.0.0.1 на 0.0.0.0 для прослушивания всех IP

```{.yaml linenums="1"}
external-controller: 127.0.0.1:9090
```

Настройка заголовков CORS для API

```{.yaml linenums="1"}
external-controller-cors:
  allow-origins:
    - '*'
  allow-private-network: true
```

Unix socket API прослушивающий адрес

!!! warning ""
    При доступе к API через Unix socket проверка secret не выполняется, обеспечьте безопасность самостоятельно

```{.yaml linenums="1"}
external-controller-unix: mihomo.sock
```

Windows namedpipe API прослушивающий адрес

!!! warning ""
    При доступе к API через Windows namedpipe проверка secret не выполняется, обеспечьте безопасность самостоятельно

```{.yaml linenums="1"}
external-controller-pipe: \\.\pipe\mihomo
```

HTTPS-API прослушивающий адрес, требует настройки сертификата и приватного ключа в разделе `tls`, для использования TLS также необходимо указать `external-controller`

```{.yaml linenums="1"}
external-controller-tls: 127.0.0.1:9443
```

Запуск DOH-сервера на порту RESTful API

!!! warning ""
    Этот URL не проверяет secret, обеспечьте безопасность самостоятельно

```{.yaml linenums="1"}
external-doh-server: /dns-query
```

Ключ доступа к API

```{.yaml linenums="1"}
secret: ""
```

## Внешний пользовательский интерфейс

Позволяет запускать статические веб-ресурсы (например, Clash-dashboard) через Clash API, путь доступа - адрес API/ui

```{.yaml linenums="1"}
external-ui: /path/to/ui/folder
```

Может быть абсолютным путем или относительным путем к рабочему каталогу Clash

!!! note Обратите внимание: если путь не находится в рабочем каталоге Clash, вручную задайте переменную среды SAFE_PATHS, чтобы добавить его в безопасный путь. Синтаксис этой переменной среды совпадает с правилом анализа переменной среды PATH этой операционной системы (то есть он разделяется точкой с запятой в Windows и двоеточием в других системах).

## Настройка имени внешнего пользовательского интерфейса

```{.yaml linenums="1"}
external-ui-name: xd      #  объединяется как external-ui/xd
```

Необязательно, при обновлении будет использоваться указанная папка, если не настроено, то обновляется непосредственно в каталог external-ui

## Настройка URL для загрузки внешнего пользовательского интерфейса

```{.yaml linenums="1"}
external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip" #загрузка из ветки GitHub Pages
```

## Кэширование

```{.yaml linenums="1"}
profile:
  store-selected: true
  # Сохранение выбора API для групп политик для использования при следующем запуске
  store-fake-ip: true
  # Сохранение таблицы сопоставления fakeip, при повторном подключении домена используется ранее назначенный адрес
```

## Унифицированная задержка

При включении унифицированной задержки рассчитывается RTT для устранения различий задержки между различными типами узлов из-за рукопожатия соединения

Возможные значения: `true/false`

```{.yaml linenums="1"}
unified-delay: true
```

## TCP одновременность

Возможные значения: `true/false`

```{.yaml linenums="1"}
tcp-concurrent: true
```

## Интерфейс исходящего соединения

Интерфейс для исходящего трафика Clash

```{.yaml linenums="1"}
interface-name: en0
```

## Метка маршрутизации

Предоставляет метку трафика по умолчанию для исходящих соединений в Linux

```{.yaml linenums="1"}
routing-mark: 6666
```

## TLS

В настоящее время используется только для HTTPS API

```{.yaml linenums="1"}
tls:
  certificate: string # Сертификат в формате PEM или путь к сертификату
  private-key: string # Приватный ключ в формате PEM или путь к приватному ключу
```

## Глобальный отпечаток клиента

Глобальный отпечаток TLS, приоритет ниже, чем client-fingerprint внутри proxy.

В настоящее время поддерживается для TCP/grpc/WS/HTTP с включенным TLS, поддерживаемые протоколы: VLESS, Vmess и trojan.

```{.yaml linenums="1"}
global-client-fingerprint: chrome
```

!!! note
    Возможные значения: `chrome`, `firefox`, `safari`, `iOS`, `android`, `edge`, `360`, `qq`, `random`. При выборе `random` генерируется отпечаток современного браузера на основе данных вероятности Cloudflare Radar.

## Режим данных GEOIP

Изменение файла, используемого для geoip: mmdb или dat. Возможные значения: `true`/`false`, `true` для dat, по умолчанию `false`

```{.yaml linenums="1"}
geodata-mode: true 
```

## Режим загрузки GEO файлов

Доступные режимы загрузки:

* `standard`: стандартный загрузчик
* `memconservative`: загрузчик, оптимизированный для устройств с ограниченной памятью (значение по умолчанию)

```{.yaml linenums="1"}
geodata-loader: memconservative
```

## Автоматическое обновление GEO

```{.yaml linenums="1"}
geo-auto-update: false
```

Интервал обновления в часах

```{.yaml linenums="1"}
geo-update-interval: 24
```

## Настройка URL для загрузки GEO

```{.yaml linenums="1"}
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

## Настройка глобального UA

Настройка User-Agent для загрузки внешних ресурсов, по умолчанию `clash.meta`

```{.yaml linenums="1"}
global-ua: clash.meta
```

## Поддержка ETag

Поддержка ETag для загрузки внешних ресурсов, по умолчанию `true`

```{.yaml linenums="1"}
etag-support: true
``` 
