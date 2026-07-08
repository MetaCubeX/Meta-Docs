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
=== "mkcp"
    ```{.yaml linenums="1"}
    proxies:
    - name: "mkcp-opts-example"
      type: vmess
      server: server
      port: 443
      uuid: uuid
      alterId: 32
      cipher: auto
      network: mkcp
      mkcp-opts:
        mtu: 1350
        tti: 50
        uplink-capacity: 5
        downlink-capacity: 20
        congestion: false
        write-buffer: 2097152
        read-buffer: 2097152
        seed: ""
        header: ""
    ```
=== "mekya"
    ```{.yaml linenums="1"}
    proxies:
    - name: "mekya-opts-example"
      type: vmess
      server: server
      port: 443
      uuid: uuid
      alterId: 32
      cipher: auto
      tls: true
      network: mekya
      mekya-opts:
        url: https://server:443/mekya
        max-write-delay: 80
        max-request-size: 96000
        polling-interval-initial: 200
        h2-pool-size: 8
        kcp:
          mtu: 1350
          tti: 15
          uplink-capacity: 40
          downlink-capacity: 2000
          congestion: false
          write-buffer: 67108864
          read-buffer: 67108864
          seed: ""
          header: ""
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
      alpn: [h2] # By default, only h2 mode is supported. To enable h3 mode, you need to set alpn: [h3]; to enable HTTP/1.1 mode, you need to set alpn: [http/1.1].
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
        # x-padding-obfs-mode: false
        # x-padding-key: x_padding
        # x-padding-header: Referer
        # x-padding-placement: queryInHeader # Available: queryInHeader, cookie, header, query
        # x-padding-method: repeat-x # Available: repeat-x, tokenish
        # uplink-http-method: POST # Available: POST, PUT, PATCH, DELETE
        # session-placement: path # Available: path, query, cookie, header
        # session-key: ""
        # session-table: "" # Available: "", "uuid", "ALPHABET", "Alphabet", "BASE36", "Base62", "HEX", "alphabet", "base36", "hex", "number"
        # session-length: "16-32"
        # seq-placement: path # Available: path, query, cookie, header
        # seq-key: ""
        # uplink-data-placement: body # Available: body, cookie, header
        # uplink-data-key: ""
        # uplink-chunk-size: 0 # only applicable when uplink-data-placement is not body
        # sc-max-each-post-bytes: 1000000
        # sc-min-posts-interval-ms: 30
        # reuse-settings: # aka XMUX
        #   max-concurrency: "16-32"
        #   max-connections: "0"
        #   c-max-reuse-times: "0"
        #   h-max-request-times: "600-900"
        #   h-max-reusable-secs: "1800-3000"
        #   h-keep-alive-period: 0
        # download-settings:
        #   ## xhttp part
        #   path: "/"
        #   host: xxx.com
        #   headers:
        #     X-Forwarded-For: ""
        #   reuse-settings: # aka XMUX
        #     max-concurrency: "16-32"
        #     max-connections: "0"
        #     c-max-reuse-times: "0"
        #     h-max-request-times: "600-900"
        #     h-max-reusable-secs: "1800-3000"
        #     h-keep-alive-period: 0
        #   ## proxy part
        #   server: server
        #   port: 443
        #   tls: true
        #   alpn: ...
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

## mkcp-opts

Настройки транспортного уровня `mkcp`, действуют только когда транспортный уровень — `mkcp`.

!!! note
    Транспортный уровень mKCP поддерживается только VMess. Не используйте его с другими протоколами.

### mkcp-opts.mtu

Максимальный размер передаваемого блока.

### mkcp-opts.tti

Интервал передачи, в миллисекундах.

### mkcp-opts.uplink-capacity

Пропускная способность вверх, в MB/s.

### mkcp-opts.downlink-capacity

Пропускная способность вниз, в MB/s.

### mkcp-opts.congestion

Включать ли управление перегрузкой.

### mkcp-opts.write-buffer

Размер буфера записи, в байтах.

### mkcp-opts.read-buffer

Размер буфера чтения, в байтах.

### mkcp-opts.seed

Seed для аутентификации AES-GCM. Оставьте пустым для аутентификации по умолчанию.

### mkcp-opts.header

Маскировка заголовка пакета. Возможные значения: `none`/`srtp`/`utp`/`wechat-video`/`dtls`/`wireguard`.

## mekya-opts

Настройки транспортного уровня `mekya`, действуют только когда транспортный уровень — `mekya`.

!!! note
    Транспортный уровень Mekya поддерживается только VMess. Не используйте его с другими протоколами.

### mekya-opts.url

URL сервера Mekya.

### mekya-opts.max-write-delay

Максимальное ожидание агрегации после первого пакета, в миллисекундах.

### mekya-opts.max-request-size

Максимальный размер полезной нагрузки одного HTTP-запроса, в байтах.

### mekya-opts.polling-interval-initial

Интервал пустого polling, в миллисекундах.

### mekya-opts.h2-pool-size

Размер пула соединений HTTP/2.

### mekya-opts.kcp

Внутренние параметры KCP для Mekya. Поля совпадают с [mkcp-opts](#mkcp-opts).

## xhttp-opts

Параметр транспортного уровня `xhttp` вступает в силу только тогда, когда транспортный уровень — `xhttp`.

По умолчанию поддерживается только режим h2. Чтобы включить режим h3, необходимо установить параметр alpn: [h3]; чтобы включить режим HTTP/1.1, необходимо установить параметр alpn: [http/1.1].

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

### xhttp-opts.x-padding-obfs-mode

Включает обфускацию отступов. По умолчанию — false для обратной совместимости.

### xhttp-opts.x-padding-key

Ключ, используемый для хранения значений отступов. Его значение зависит от `x-padding-placement`:

* Имя параметра запроса в URL (если placement — `queryInHeader`)
* Имя cookie
* Имя заголовка HTTP
* Имя параметра запроса URL

### xhttp-opts.x-padding-header

Имя заголовка HTTP. Актуально только если `x-padding-placement` — `header` или `queryInHeader`.

### xhttp-opts.x-padding-placement

Определяет место размещения значений отступов. Возможные значения: `queryInHeader`, `cookie`, `header`, `query`. Действует только при значении `x-padding-obfs-mode` равном true.

### xhttp-opts.x-padding-method

Определяет способ генерации значения заполнения

* `repeat-x`: Метод по умолчанию (длинная последовательность из X символов)
* `tokenish`: Генерирует случайный токен Base62

### xhttp-opts.uplink-http-method

Изменяет метод HTTP, используемый для передачи данных восходящего канала. Используйте любой метод, поддерживающий тело запроса (например, `PUT`, `PATCH`) и разрешенный CDN.

### xhttp-opts.session-placement

Расположение идентификатора сессии. Варианты: `path`, `query`, `cookie`, `header`

### xhttp-opts.session-key

Имя ключа для идентификатора сессии (не применяется, если местоположение размещения — `path`)

### xhttp-opts.session-table

Таблица символов для генерации идентификатора сессии. Возможные значения: `""`, `uuid`, `ALPHABET`, `Alphabet`, `BASE36`, `Base62`, `HEX`, `alphabet`, `base36`, `hex`, `number`.

### xhttp-opts.session-length

Диапазон длины идентификатора сессии. Начальное значение не может быть `0`; общее пространство ID должно быть больше 2,1 млрд. Действует только если `session-table` не пустой или равен `uuid`.

### xhttp-opts.seq-placement

Расположение порядкового номера. Варианты: `path`, `query`, `cookie`, `header`. Если `session-placement` установлено на `path`, то `seq-placement` также должно быть установлено на `path`.

### xhttp-opts.seq-key

Имя ключа для порядкового номера (не применяется, если место размещения — `path`).

### xhttp-opts.uplink-data-placement

Место для размещения разделенных фрагментов данных восходящего канала (применимо только в режиме `packet-up`). Варианты: `cookie` или `header`

### xhttp-opts.uplink-data-key

Базовое имя ключа, используемого для передачи фрагментов данных. Клиент автоматически разбивает данные на несколько частей, а сервер собирает их заново.

### xhttp-opts.uplink-chunk-size

Максимальный размер каждого блока данных восходящего канала (в байтах) (применяется только если `uplink-data-placement` не равен `body`)

Минимальный: 64 байта

Рекомендуемый:

* Для размещения `cookie`: 3072 байта (по умолчанию 3 КБ)
* Для размещения `header`: 4096 байт (по умолчанию 4 КБ)

Если не указано, соответствующее значение по умолчанию будет выбрано автоматически на основе `uplink-data-placement`.

### xhttp-opts.sc-max-each-post-bytes

Максимальный объем данных, который может передавать каждый POST-запрос клиента (значение по умолчанию). 1000000 эквивалентно 1 МБ. Это значение должно быть меньше максимального объема, разрешенного HTTP-посредниками, такими как CDN. Сервер также будет отклонять POST-запросы, превышающие это значение.

### xhttp-opts.sc-min-posts-interval-ms

Минимальный интервал между POST-запросами, инициированными клиентом, на основе одного прокси-запроса. Значение по умолчанию — 30 миллисекунд.

### xhttp-opts.reuse-settings

Настройки повторного использования соединений (например, XMUX)

Примечание: В отличие от исходной реализации, этот параметр не имеет значения по умолчанию. Если он не указан, повторное использование соединений не будет включено, то есть каждый раз будет открываться новое базовое соединение.

### xhttp-opts.reuse-settings.max-concurrency

Максимальное количество одновременных запросов к прокси на одно базовое соединение. Как только количество запросов к прокси в соединении достигнет этого значения, будет установлено новое соединение для обработки большего количества запросов к прокси.

### xhttp-opts.reuse-settings.max-connections

Максимальное количество одновременных соединений. До достижения этого значения каждый новый запрос к прокси будет открывать новое соединение. После этого будут повторно использоваться существующие соединения.

Это значение конфликтует с параметром max-concurrency; можно выбрать только один из них.

### xhttp-opts.reuse-settings.c-max-reuse-times

Максимальное количество раз, когда соединение может быть повторно использовано. После этого количества повторных использований новые запросы к прокси-серверу назначаться не будут, и соединение будет закрыто после закрытия последнего внутреннего запроса к прокси-серверу.

### xhttp-opts.reuse-settings.h-max-request-times

Максимальное количество раз, когда может быть передано одно соединение. Этот параметр неточен, и запросы GET в Golang имеют автоматические повторные попытки, поэтому заполнять его не рекомендуется.

### xhttp-opts.reuse-settings.h-max-reusable-secs

По истечении этого времени новые HTTP-запросы не будут назначаться соединению TCP/QUIC, и соединение будет закрыто после закрытия последнего внутреннего HTTP-запроса.

### xhttp-opts.reuse-settings.h-keep-alive-period

Этот параметр задает интервал в секундах, в течение которого клиент отправляет пакет keep-alive, когда соединение H2/H3 находится в режиме ожидания. Значение по умолчанию — 0, что эквивалентно 45 секундам для Chrome H2 или 10 секундам для quic-go H3. Это единственный параметр в XMUX, который не допускает диапазон значений (характеристика этого параметра — случайность) и допускает отрицательные числа (например, -1 для отключения пакетов keep-alive в режиме ожидания). Рекомендуется оставить значение 0.

### xhttp-opts.download-settings

Настройки разделения загрузки/выгрузки

Примечание: Этот параметр переопределяет исходную конфигурацию. Если какой-либо параметр не указан, будут использоваться параметры загрузки. (Поддерживаются только параметры, указанные в экземпляре; параметры, не указанные в списке, не будут переопределены.)
