# Sudoku

```{.yaml linenums="1"}
proxies:
  - name: sudoku
    type: sudoku
    server: 1.2.3.4
    port: 443
    key: "<client_key>"
    aead-method: chacha20-poly1305
    padding-min: 2
    padding-max: 7
    table-type: prefer_ascii
    # custom-table: xpxvvpvv
    # custom-tables: ["xpxvvpvv", "vxpvxvvp"]
    # multiplex: "off"
    httpmask:
      disable: false
      mode: legacy
      tls: true
      mask-host: ""
      path-root: ""
      multiplex: off
    enable-pure-downlink: false
```

[Общие поля](./index.ru.md)

## key

Если вы используете пару ключей ED25519, сгенерированную sudoku, укажите здесь приватный ключ из пары, в противном случае используйте тот же UUID, что и на сервере

## aead-method

Возможные значения: `chacha20-poly1305`, `aes-128-gcm`, `none` - мы гарантируем безопасность даже при использовании none благодаря слою обфускации sudoku

## padding-min

Минимальное количество байтов заполнения

## padding-max

Максимальное количество байтов заполнения

## table-type

Возможные значения: prefer_ascii, prefer_entropy - первый использует полное ASCII отображение, второй гарантирует энтропию (Хэмминг 1) менее 3

## custom-table

Опционально, пользовательская раскладка байтов, должна содержать 2x, 2p, 4v в любой комбинации. При включении этого параметра нужно настроить `table-type` как `prefer_entropy`

## custom-tables

Опционально, список пользовательских раскладок байтов (x/v/p) для ротации в режиме xvp; при наличии переопределяет custom-table

## multiplex

Необязательно: `off` (по умолчанию), `auto` (повторно использует только нижележащее соединение HTTPMask), `on` (включает Sudoku mux с несколькими целями в одном сеансе поверх raw TCP или HTTPMask).

## httpmask.disable

Включить ли HTTP маску

## httpmask.mode

Опционально: legacy (по умолчанию), stream, poll, auto; stream/poll/auto поддерживают работу через CDN/прокси

## httpmask.tls

Опционально: действует только при http-mask-mode в режиме stream/poll/auto; true принудительно включает https; false принудительно включает http (не определяется автоматически по порту)

## httpmask.host

Опционально: переопределяет Host/SNI (поддерживает example.com или example.com:443); действует только при http-mask-mode в режиме stream/poll/auto

## httpmask.path-root

Опционально: префикс пути первого уровня для HTTP туннеля (должен совпадать на обеих сторонах), например "aabbcc" => /aabbcc/session, /aabbcc/stream, /aabbcc/api/v1/upload

## httpmask.multiplex

Параметр для совместимости со старой конфигурацией. Если задан, имеет приоритет над верхнеуровневым `multiplex`.

## enable-pure-downlink

Включить ли обфускацию нисходящего канала, при false значительно повышает скорость загрузки с сохранением безопасности данных, должно совпадать с сервером (если здесь false, то aead не может быть none)
