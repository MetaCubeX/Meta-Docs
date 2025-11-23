# Конфигурация транспортного уровня

=== "http"
    ```{.yaml linenums="1"}
    proxies:
    - name: "http-opts-example"
      type: xxxxx
      network: http
      http-opts:
        method: "GET"
        path:
        - '/'
        - '/video'
        headers:
          Connection:
          - keep-alive
    ```

=== "h2"
    ```{.yaml linenums="1"}
    proxies:
    - name: "h2-opts-example"
      type: xxxxx
      network: h2
      h2-opts:
        host:
        - example.com
        path: /
    ```

=== "grpc"
    ```{.yaml linenums="1"}
    proxies:
    - name: "grpc-opts-example"
      type: xxxxx
      network: grpc
      grpc-opts:
        grpc-service-name: example
    ```
=== "ws"
    ```{.yaml linenums="1"}
    proxies:
    - name: "ws-opts-example"
      type: xxxxx
      network: ws
      ws-opts:
        path: /path
        headers:
          Host: example.com
        max-early-data:
        early-data-header-name:
        v2ray-http-upgrade: false
        v2ray-http-upgrade-fast-open: false
    ```

=== "xhttp"
    ```{.yaml linenums="1"}
    proxies:
    - name: "xhttp-opts-example"
      type: vless # поддерживается для vless, vmess, trojan
      network: xhttp
      xhttp-opts:
        host: example.org
        path: /splithttp
        mode: packet-up
        http-version: "3"
        headers:
          User-Agent: "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36"
        x-padding-bytes:
          from: 100
          to: 1000
        sc-max-each-post-bytes:
          from: 1000000
          to: 1000000
        sc-min-posts-interval-ms:
          from: 30
          to: 30
        sc-stream-up-server-secs:
          from: 25
          to: 60
        xmux:
          max-concurrency:
            from: 8
            to: 16
          max-connections: 0
          h-keep-alive-period: 15
          h-max-request-times:
            from: 100
            to: 200
          h-max-reusable-secs:
            from: 1800
            to: 3000
        # download-settings: # необязательно, отдельные настройки для загрузки
        #   path: /download
        #   mode: stream-up
    ```

## http-opts

Настройки транспортного уровня `http`, действуют только когда транспортный уровень - `http`

### http-opts.method

Метод HTTP-запроса

### http-opts.path

Путь HTTP-запроса

### http-opts.headers

Заголовки HTTP-запроса

!!! note
    Транспортный уровень H2 в Mihomo не реализует функцию мультиплексирования. Если вам нужно мультиплексирование, в Mihomo рекомендуется использовать протокол gRPC или sing-mux

## h2-opts

Настройки транспортного уровня `h2`, действуют только когда транспортный уровень - `h2`

### h2-opts.host

Список доменных имен хостов, если указан, клиент выберет случайным образом, сервер будет проверять

### h2-opts.path

Путь HTTP-запроса

## grpc-opts

Настройки транспортного уровня `grpc`, действуют только когда транспортный уровень - `grpc`

### grpc-opts.grpc-service-name

Имя сервиса gRPC

## ws-opts

Настройки транспортного уровня `ws`, действуют только когда транспортный уровень - `ws`

### ws-opts.path

Путь запроса

### ws-opts.headers

Заголовки запроса

### ws-opts.max-early-data

Порог длины первого пакета Early Data

### ws-opts.early-data-header-name

Имя заголовка для Early Data

### ws-opts.v2ray-http-upgrade

Использовать HTTP upgrade

### ws-opts.v2ray-http-upgrade-fast-open

Включить fast open для HTTP upgrade

## xhttp-opts

Настройки транспортного уровня `xhttp`, действуют только когда транспортный уровень - `xhttp`

XHTTP — это транспортный протокол, использующий HTTP для передачи данных. Поддерживает HTTP/1.1, HTTP/2 и HTTP/3 (QUIC). Включает мультиплексирование соединений, различные режимы работы и настройки обфускации трафика.

Совместим с протоколами: VLESS, VMess, Trojan

### xhttp-opts.host

**Тип:** `string`
**Значение по умолчанию:** автоопределение

HTTP заголовок Host. Если не указан, будет использовано значение из поля `server`

### xhttp-opts.path

**Тип:** `string`
**Значение по умолчанию:** `/`

Базовый путь для HTTP запросов. Автоматически нормализуется к формату `/path/`

Пример: `/splithttp` → `/splithttp/`

### xhttp-opts.mode

**Тип:** `string`
**Значение по умолчанию:** `packet-up`
**Допустимые значения:** `auto`, `packet-up`, `stream-up`, `stream-one`

Режим работы соединения:

- `packet-up` — данные разбиваются на пакеты, каждый отправляется отдельным POST запросом.
- `stream-up` — двунаправленный стрим с отдельным потоком для загрузки данных
- `stream-one` — одно соединение для загрузки и выгрузки данных
- `auto` — автоматический выбор режима. Предпочитает stream, если настройки download отличаются или присутствует reality конфигурация

### xhttp-opts.headers

**Тип:** `map[string]string`
**Значение по умолчанию:** пусто

Пользовательские HTTP заголовки для запросов. Полезно для имитации браузера

Пример:
```yaml
headers:
  User-Agent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
  Accept: "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
  Accept-Language: "en-US,en;q=0.5"
```

### xhttp-opts.http-version

**Тип:** `string`
**Значение по умолчанию:** автоопределение (1.1 для HTTP, 2 для HTTPS)
**Допустимые значения:** `1.1`, `2`, `3`, `h3`

Версия HTTP протокола:

- `1.1` — HTTP/1.1
- `2` — HTTP/2 (требует TLS)
- `3` или `h3` — HTTP/3 через QUIC (требует TLS)

### xhttp-opts.no-grpc-header

**Тип:** `boolean`
**Значение по умолчанию:** `false`

Отключить автоматическое добавление заголовка `Content-Type: application/grpc`

### xhttp-opts.no-sse-header

**Тип:** `boolean`
**Значение по умолчанию:** `false`

Отключить автоматическое добавление заголовка `Content-Type: text/event-stream` для Server-Sent Events

### xhttp-opts.x-padding-bytes

**Тип:** `Range` (число или диапазон)
**Значение по умолчанию:** `100-1000`

Количество байтов padding, добавляемых в заголовок Referer в качестве параметра запроса `x_padding` (заполняется символами 'X')

Поддерживаемые форматы:
```yaml
# Одно число
x-padding-bytes: 500

# Диапазон (случайное значение от from до to)
x-padding-bytes:
  from: 100
  to: 1000
```

Используется для обфускации размера запросов

### xhttp-opts.sc-max-each-post-bytes

**Тип:** `Range`
**Значение по умолчанию:** `1000000-1000000` (1 МБ)

Максимальное количество байт, отправляемых в одном POST запросе в режиме `packet-up`

### xhttp-opts.sc-min-posts-interval-ms

**Тип:** `Range`
**Значение по умолчанию:** `30-30`

Минимальный интервал в миллисекундах между последовательными POST запросами в режиме `packet-up`

### xhttp-opts.sc-stream-up-server-secs

**Тип:** `Range`
**Значение по умолчанию:** не установлено

Время жизни потокового соединения в секундах для режима `stream-up`. После истечения времени соединение переподключается

### xhttp-opts.xmux

**Тип:** `object`

Конфигурация мультиплексирования HTTP соединений. Позволяет повторно использовать соединения и управлять их жизненным циклом

#### xhttp-opts.xmux.max-concurrency

**Тип:** `Range`
**Значение по умолчанию:** `1-1`

Максимальное количество одновременных запросов на одно HTTP соединение

Пример:
```yaml
max-concurrency:
  from: 8
  to: 16
```

#### xhttp-opts.xmux.max-connections

**Тип:** `Range`
**Значение по умолчанию:** `0` (без ограничений)

Максимальное общее количество HTTP соединений, которые можно поддерживать одновременно

#### xhttp-opts.xmux.c-max-reuse-times

**Тип:** `Range`
**Значение по умолчанию:** не установлено

Лимит повторного использования соединения (количество использований)

#### xhttp-opts.xmux.h-max-request-times

**Тип:** `Range`
**Значение по умолчанию:** `600-900`

Максимальное количество запросов на одно HTTP соединение перед его закрытием

#### xhttp-opts.xmux.h-max-reusable-secs

**Тип:** `Range`
**Значение по умолчанию:** `1800-3000` (30-50 минут)

Максимальное время жизни HTTP соединения в секундах

#### xhttp-opts.xmux.h-keep-alive-period

**Тип:** `int`
**Значение по умолчанию:** `30`

Период keep-alive для HTTP соединений в секундах

### xhttp-opts.download-settings

**Тип:** `Config` (объект xhttp-opts)
**Значение по умолчанию:** не установлено

Отдельная конфигурация xhttp для направления загрузки данных. Позволяет использовать разные настройки для входящего трафика

Пример:
```yaml
download-settings:
  path: /download
  mode: stream-up
  http-version: "2"
```

!!! note "Примечание о типе Range"
    Тип `Range` используется для рандомизации параметров и поддерживает несколько форматов:

    - Одно число: `100` (значение всегда будет 100)
    - Диапазон: `from: 100, to: 1000` (случайное значение от 100 до 1000)
    - Строка: `"100-1000"` (эквивалентно диапазону)