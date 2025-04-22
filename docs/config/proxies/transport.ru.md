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