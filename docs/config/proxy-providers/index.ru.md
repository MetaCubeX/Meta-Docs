# Поставщики прокси

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    proxy: DIRECT
    size-limit: 0
    header:
      User-Agent:
      - "Clash/v1.18.0"
      - "mihomo/1.18.3"
      Authorization:
      - 'token 1231231'
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
      timeout: 5000
      lazy: true
      expected-status: 204
    override:
      tfo: false
      mptcp: false
      udp: true
      udp-over-tcp: false
      down: "50 Mbps"
      up: "10 Mbps"
      skip-cert-verify: true
      dialer-proxy: proxy
      interface-name: tailscale0
      routing-mark: 233
      ip-version: ipv4-prefer
      additional-prefix: "provider1 prefix |"
      additional-suffix: "| provider1 suffix"
      proxy-name:
      - pattern: "IPLC-(.*?)倍"
        target: "iplc x $1"
    filter: "(?i)港|hk|hongkong|hong kong"
    exclude-filter: "xxx"
    exclude-type: "ss|http"
    payload:
      - name: "ss1"
        type: ss
        server: server
        port: 443
        cipher: chacha20-ietf-poly1305
        password: "password"
```

## name

Обязательно, например `provider1`, должно быть уникальным. Рекомендуется не дублировать имена с [группами политик](../proxy-groups/index.md#name).

## type

Обязательно, тип `provider`, варианты: `http` / `file` / `inline`.

## url

Если тип `http`, это поле должно быть настроено.

## path

Необязательно, путь к файлу, должен быть уникальным. Если не указан, для имени файла будет использован MD5 от URL.

По соображениям безопасности этот путь ограничен и допускается только в пределах `HomeDir` (настраивается параметром запуска -d). Если вы хотите хранить его в любом месте, установите переменную окружения `SKIP_SAFE_PATH_CHECK=1`.

## interval

Время обновления `provider`, измеряется в секундах.

## proxy

Загрузка/обновление через указанный прокси.

## size-limit

Ограничение максимального размера загружаемых файлов, по умолчанию 0, что означает отсутствие ограничения размера; единица измерения - байты (`b`)

## header

Пользовательские HTTP-заголовки запроса.

## health-check

Проверка работоспособности (тестирование задержки).

### health-check.enable

Включение функции, варианты `true/false`.

### health-check.url

Адрес проверки работоспособности, рекомендуется использовать один из следующих адресов:

=== "Cloudflare"
    ```yaml
    https://cp.cloudflare.com
    ```

=== "Google"
    ```yaml
    https://www.gstatic.com/generate_204
    ```

### health-check.interval

Интервал проверки работоспособности, измеряется в секундах.

### health-check.timeout

Таймаут проверки работоспособности, измеряется в миллисекундах.

### health-check.lazy

Ленивое состояние, по умолчанию `true`, тестирование не выполняется, когда этот узел провайдера не используется.

### health-check.expected-status

См. [ожидаемый статус](../proxy-groups/index.md#expected-status).

## override

Переопределение содержимого узла, поддерживаются следующие поля.

### override.additional-prefix

Добавление фиксированного префикса к имени узла.

### override.additional-suffix

Добавление фиксированного суффикса к имени узла.

### override.proxy-name

Замена содержимого имени узла, поддерживает регулярные выражения, где pattern - содержимое для замены, а target - цель замены.

### override.Прочие_параметры_конфигурации

См. общие поля [tfo](../proxies/index.md#tfo)

См. общие поля [mptcp](../proxies/index.md#mptcp)

См. общие поля [udp](../proxies/index.md#udp).

См. `Shadowsocks` [udp-over-tcp](../proxies/ss.md#udp-over-tcp)

См. `Hysteria`/`Hysteria2` [up](../proxies/hysteria2.md#updown).

См. `Hysteria`/`Hysteria2` [down](../proxies/hysteria2.md#updown).

См. общие поля [skip-cert-verify](../proxies/tls.md#skip-cert-verify).

См. общие поля [dialer-proxy](../proxies/index.md#dialer-proxy).

См. общие поля [interface-name](../proxies/index.md#interface-name).

См. общие поля [routing-mark](../proxies/index.md#routing-mark).

См. общие поля [ip-version](../proxies/index.md#ip-version).

## filter

Фильтрация узлов, соответствующих ключевым словам или [регулярным выражениям](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md), несколько регулярных выражений можно разделить символом `.

## exclude-filter

Исключение узлов, соответствующих ключевым словам или [регулярным выражениям](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md), несколько регулярных выражений можно разделить символом `.

## exclude-type

Не поддерживает регулярные выражения; используйте `|` для разделения и исключения на основе типа узла.

`exclude-type` провайдера использует `type` из конфигурационного файла для исключения

## payload

Содержимое, действует только когда `type` имеет значение `inline` 