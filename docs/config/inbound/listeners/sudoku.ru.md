# Sudoku

```{.yaml linenums="1"}
listeners:
- name: sudoku-in-1
  type: sudoku
  port: 8443 # Поддерживается только один порт
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
  key: "<server_key>" # Если вы используете пару ключей ED25519, сгенерированную sudoku, здесь указывается открытый ключ из пары, конечно, вы также можете использовать любой UUID в качестве ключа
  aead-method: chacha20-poly1305 # Варианты: chacha20-poly1305, aes-128-gcm, none (не рекомендуется; none не обеспечивает защиту AEAD)
  padding-min: 1 # Минимальный коэффициент заполнения (0-100)
  padding-max: 15 # Максимальный коэффициент заполнения (0-100, должен быть >= padding-min)
  table-type: prefer_ascii # Варианты: prefer_ascii, prefer_entropy, up_ascii_down_entropy, up_entropy_down_ascii
  # custom-table: xpxvvpvv # Пользовательская раскладка из 2 x, 2 p и 4 v; действует только для направления entropy
  # custom-tables: ["xpxvvpvv", "vxpvxvvp"] # Список раскладок для ротации; если не пуст, переопределяет custom-table
  handshake-timeout: 5 # Опционально, в секундах
  enable-pure-downlink: false # false = оптимизированный по пропускной способности режим; true = чистый нисходящий канал Sudoku
  httpmask:
    disable: false # true отключает всю HTTP-маскировку и туннелирование
    mode: legacy # legacy (по умолчанию), stream, poll, auto, ws
    # path-root: "" # Префикс пути HTTP-туннеля должен совпадать на обеих сторонах
  # При отклонении HTTPMask исходные данные можно передать другому сервису на том же порту:
  # fallback: "127.0.0.1:80"
  # Совместимые варианты записи параметров объекта httpmask:
  # disable-http-mask: false
  # http-mask-mode: legacy
  # path-root: ""
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # по умолчанию в Mbps
  #     down: 1000

```

[Общие поля](./index.ru.md)
