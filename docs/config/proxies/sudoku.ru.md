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
      host: ""
      path-root: ""
      multiplex: "off"
    enable-pure-downlink: false
```

[Общие поля](./index.ru.md)

## key

Если вы используете пару ключей ED25519, сгенерированную sudoku, укажите здесь приватный ключ из пары, в противном случае используйте тот же UUID, что и на сервере

## aead-method

Возможные значения: `chacha20-poly1305`, `aes-128-gcm`, `none`. Использование `none` не рекомендуется, поскольку этот режим не обеспечивает защиту AEAD.

## padding-min

Минимальный коэффициент заполнения от 0 до 100.

## padding-max

Максимальный коэффициент заполнения от 0 до 100. Он должен быть не меньше `padding-min`.

## table-type

Возможные значения: `prefer_ascii`, `prefer_entropy`, `up_ascii_down_entropy`, `up_entropy_down_ascii`.

## custom-table

Опциональная пользовательская раскладка байтов. Она должна содержать 2 `x`, 2 `p` и 4 `v` в любой комбинации и действует только для направления entropy.

## custom-tables

Опционально, список пользовательских раскладок байтов (x/v/p) для ротации в режиме xvp; при наличии переопределяет custom-table

## multiplex

Необязательно: `off` (по умолчанию), `auto` (повторно использует только нижележащее соединение HTTPMask), `on` (включает Sudoku mux с несколькими целями в одном сеансе поверх raw TCP или HTTPMask).

## httpmask.disable

Отключать ли всю HTTP-маскировку и туннелирование.

## httpmask.mode

Опционально: `legacy` (по умолчанию), `stream`, `poll`, `auto`, `ws`. Режимы `stream`/`poll`/`auto`/`ws` поддерживают работу через CDN или обратный прокси.

## httpmask.tls

Опционально: действует только в режимах `stream`/`poll`/`auto`/`ws`; `true` принудительно включает HTTPS, а `false` - HTTP без автоматического определения по порту.

## httpmask.host

Опционально: переопределяет Host/SNI (поддерживает `example.com` или `example.com:443`); действует только в режимах `stream`/`poll`/`auto`/`ws`.

## httpmask.path-root

Опционально: префикс пути первого уровня для HTTP-туннеля (должен совпадать на обеих сторонах), например `aabbcc` => `/aabbcc/session`, `/aabbcc/stream`, `/aabbcc/api/v1/upload`, `/aabbcc/ws`.

## httpmask.multiplex

Параметр для совместимости со старой конфигурацией. Если задан, имеет приоритет над верхнеуровневым `multiplex`.

## enable-pure-downlink

Выбирает режим нисходящего канала: `false` использует оптимизированный по пропускной способности режим, а `true` - чистый нисходящий канал Sudoku. Настройка должна совпадать с сервером.
