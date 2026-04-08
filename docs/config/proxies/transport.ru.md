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
        # grpc-user-agent:
        # ping-interval: 0 
        # max-connections: 1
        # min-streams: 0
        # max-streams: 0
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
      type: vless
      server: server
      port: 443
      uuid: uuid
      udp: true
      tls: true
      network: xhttp
      alpn:
        - h2
      # ech-opts: ...
      # reality-opts: ...
      # skip-cert-verify: false
      # fingerprint: ...
      # certificate: ...
      # private-key: ...
      servername: xxx.com
      client-fingerprint: chrome
      encryption: ""
      xhttp-opts:
        path: "/"
        host: xxx.com
        # mode: "stream-one" # Available: "stream-one", "stream-up" or "packet-up"
        # headers:
        #   X-Forwarded-For: ""
        # no-grpc-header: false
        # x-padding-bytes: "100-1000"
        # sc-max-each-post-bytes: 1000000
        # reuse-settings: # aka XMUX
        #   max-connections: "16-32"
        #   max-concurrency: "0"
        #   c-max-reuse-times: "0"
        #   h-max-request-times: "600-900"
        #   h-max-reusable-secs: "1800-3000"
        # download-settings:
        #   ## xhttp part
        #   path: "/"
        #   host: xxx.com
        #   headers:
        #     X-Forwarded-For: ""
        #   no-grpc-header: false
        #   x-padding-bytes: "100-1000"
        #   sc-max-each-post-bytes: 1000000
        #   reuse-settings: # aka XMUX
        #     max-connections: "16-32"
        #     max-concurrency: "0"
        #     c-max-reuse-times: "0"
        #     h-max-request-times: "600-900"
        #     h-max-reusable-secs: "1800-3000"
        #   ## proxy part
        #   server: server
        #   port: 443
        #   tls: true
        #   alpn:
        #     - h2
        #   ech-opts: ...
        #   reality-opts: ...
        #   skip-cert-verify: false
        #   fingerprint: ...
        #   certificate: ...
        #   private-key: ...
        #   servername: xxx.com
        #   client-fingerprint: chrome
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

### grpc-opts.grpc-user-agent

gRPC UserAgent

### grpc-opts.ping-interval

Интервал пульсации gRPC (по умолчанию отключен), единица измерения — секунды.

### grpc-opts.max-connections

Максимальное количество соединений. По умолчанию равно 1, что означает использование только одного базового соединения.

Конфликтует с `max-streams`.

### grpc-opts.min-streams

Минимальное количество мультиплексированных потоков в соединении до открытия нового соединения.

Конфликтует с `max-streams`.

### grpc-opts.max-streams

Максимальное количество мультиплексированных потоков в соединении до открытия нового соединения.

Конфликтует как с `max-connections`, так и с `min-streams`.

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

Параметр транспортного уровня `xhttp` вступает в силу только тогда, когда транспортный уровень — `xhttp`.

!!! note
    VLESS поддерживает только транспортный уровень xhttp; пожалуйста, не используйте его с другими протоколами.

### xhttp-opts.path

Путь запроса

### xhttp-opts.host

Имя хоста

### xhttp-opts.mode

Режим, параметры: `auto`, `stream-one`, `stream-up` или `packet-up`

### xhttp-opts.headers

Заголовки запроса

### xhttp-opts.no-grpc-header

Указывает, следует ли отправлять заголовок `Content-Type: application/grpc` для маскировки под gRPC для запросов stream-up/one.

### xhttp-opts.x-padding-bytes

Заголовки запроса клиента по умолчанию включают `Referer: ...?x_padding=XXX...`, с длиной по умолчанию от 100 до 1000 байт, рандомизированной для каждого запроса. Сервер проверяет, находится ли она в допустимом диапазоне.

### xhttp-opts.sc-max-each-post-bytes

Максимальный объем данных, который может передавать каждый POST-запрос клиента (значение по умолчанию). 1000000 эквивалентно 1 МБ. Это значение должно быть меньше максимального объема, разрешенного HTTP-посредниками, такими как CDN. Сервер также будет отклонять POST-запросы, превышающие это значение.

### xhttp-opts.reuse-settings

Настройки повторного использования соединений (например, XMUX)

Примечание: В отличие от исходной реализации, этот параметр не имеет значения по умолчанию. Если он не указан, повторное использование соединений не будет включено, то есть каждый раз будет открываться новое базовое соединение.

### xhttp-opts.reuse-settings.max-connections

Максимальное количество прокси-запросов, которые могут существовать одновременно в каждом TCP/QUIC-соединении. Как только количество прокси-запросов в соединении достигнет этого значения, ядро ​​установит новое соединение для обработки большего количества прокси-запросов.

### xhttp-opts.reuse-settings.max-concurrency

Максимальное количество одновременных соединений. До достижения этого значения каждый новый запрос к прокси-серверу будет открывать новое соединение. После этого ядро ​​начнет повторно использовать существующие соединения. Это значение конфликтует с `max-connections`, поэтому можно выбрать только одно.

### xhttp-opts.reuse-settings.c-max-reuse-times

Максимальное количество раз, когда соединение может быть повторно использовано. После этого количества повторных использований новые запросы к прокси-серверу назначаться не будут, и соединение будет закрыто после закрытия последнего внутреннего запроса к прокси-серверу.

### xhttp-opts.reuse-settings.h-max-request-times

Максимальное количество раз, когда может быть передано одно соединение. Этот параметр неточен, и запросы GET в Golang имеют автоматические повторные попытки, поэтому заполнять его не рекомендуется.

### xhttp-opts.reuse-settings.h-max-reusable-secs

По истечении этого времени новые HTTP-запросы не будут назначаться соединению TCP/QUIC, и соединение будет закрыто после закрытия последнего внутреннего HTTP-запроса.

### xhttp-opts.download-settings

Настройки разделения загрузки/выгрузки

Примечание: Этот параметр переопределяет исходную конфигурацию. Если какой-либо параметр не указан, будут использоваться параметры загрузки.
