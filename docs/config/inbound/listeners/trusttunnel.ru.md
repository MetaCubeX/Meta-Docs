# TrustTunnel

```{.yaml linenums="1"}
listeners:
- name: trusttunnel-in-1
  type: trusttunnel
  port: 10821
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (только Linux)
  # rule: sub-rule-name1 # По умолчанию использует rules; если sub-rule не найдено, используются rules
  # proxy: proxy # Если не пусто, входящий трафик передается указанному proxy; имя proxy должно быть корректным
  users:
    - username: 1
      password: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
  certificate: ./server.crt # Сертификат в формате PEM или путь к сертификату
  private-key: ./server.key # Соответствующий закрытый ключ в формате PEM или путь к нему
  network: ["tcp", "udp"] # HTTP/2 + HTTP/3
  congestion-controller: bbr
  #  bbr-profile: "" # Возможные значения: "standard", "conservative", "aggressive". По умолчанию: "standard"
  # Следующие два параметра настраивают mTLS. client-auth-cert не должен быть пустым, если client-auth-type равен "verify-if-given" или "require-and-verify"
  # client-auth-type: "" # Возможные значения: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # Сертификат в формате PEM или путь к сертификату
  # Если заполнено, включается ECH. Ключ можно создать командой mihomo generate ech-keypair <доменное-имя>
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
```

[Общие поля](./index.md)

!!! warning ""
    `certificate` и `private-key` обязательны.

## network

Если список пуст или содержит только `tcp`, поддерживается только режим HTTP/2. Добавление `udp` включает поддержку HTTP/3.

## congestion-controller

Задает алгоритм контроля перегрузки QUIC.
