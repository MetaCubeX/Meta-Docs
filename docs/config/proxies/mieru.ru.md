# Mieru

```{.yaml linenums="1"}
proxies:
  - name: mieru
    type: mieru
    server: server
    port: 2999
    port-range: 2090-2099
    transport: TCP
    username: user
    password: password
    multiplexing: MULTIPLEXING_LOW
```

[Общие поля](./index.md)

## port-range

Диапазон портов, не может использоваться одновременно с `port`

## transport

Протокол, В настоящее время поддерживает `TCP` и `UDP`.

## multiplexing

Мультиплексирование, возможные значения: `MULTIPLEXING_OFF`, `MULTIPLEXING_LOW`, `MULTIPLEXING_MIDDLE`, `MULTIPLEXING_HIGH`. `MULTIPLEXING_OFF` отключает функцию мультиплексирования. По умолчанию `MULTIPLEXING_LOW` 
