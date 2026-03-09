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
    traffic-pattern: ""
```

[Общие поля](./index.md)

## port-range

Диапазон портов, не может использоваться одновременно с `port`

## transport

Протокол, В настоящее время поддерживает `TCP` и `UDP`.

## multiplexing

Мультиплексирование, возможные значения: `MULTIPLEXING_OFF`, `MULTIPLEXING_LOW`, `MULTIPLEXING_MIDDLE`, `MULTIPLEXING_HIGH`. `MULTIPLEXING_OFF` отключает функцию мультиплексирования. По умолчанию `MULTIPLEXING_LOW` 

## traffic-pattern

Для тонкой настройки работы сети используется строка base64; формат строки можно узнать в [официальной документации mieru](https://github.com/enfein/mieru/blob/main/docs/traffic-pattern.md)

