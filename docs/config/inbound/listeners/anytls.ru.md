# AnyTLS

```{.yaml linenums="1"}
listeners:
- name: anytls-in-1
  type: anytls
  port: 10818
  listen: 0.0.0.0
  # routing-mark: 0 # устанавливает routing-mark для слушающего сокета (только для Linux)
  # Если "shadow-tls", "res-tls" и "jls-config" отключены и "allow-insecure" не равен true, необходимо заполнить "certificate" и "private-key"; не заполняйте их, если включены ShadowTLS, ResTLS или JLS
  users:
    username1: password1
    username2: password2
  certificate: ./server.crt # сертификат в формате PEM или путь к сертификату
  private-key: ./server.key # приватный ключ сертификата в формате PEM или путь к приватному ключу
  # следующие два параметра для конфигурации mTLS, если client-auth-type установлен в "verify-if-given" или "require-and-verify", то client-auth-cert не должен быть пустым
  # client-auth-type: "" # возможные значения: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # сертификат в формате PEM или путь к сертификату
  # если заполнен, то включается ech (можно сгенерировать с помощью mihomo generate ech-keypair <доменное имя в открытом виде>)
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # shadow-tls:
  #   enable: true
  #   version: 3 # поддерживает v1/v2/v3
  #   # password: shadow-tls-password # параметр конфигурации v2
  #   users: # параметр конфигурации v3
  #     - name: shadow-tls-user
  #       password: shadow-tls-password
  #   handshake:
  #     dest: www.example.com:443
  #     # proxy: ""
  # res-tls:
  #   enable: true
  #   dest: www.example.com:443
  #   password: restls-password
  #   # restls-script: ""
  #   # min-record-len: 0
  #   # proxy: ""
  #   # rate-limit: 0 # Ограничение скорости двусторонней передачи для fallback, в бит/с; 0 — без ограничений.
  # jls-config: # JLS заменяет обычный TLS; неавторизованные соединения перенаправляются на dest
  #   enable: true
  #   users:
  #     - username: jls-user
  #       password: jls-password
  #   dest: www.example.com:443
  #   # sni: www.example.com # если пусто, выводится из dest
  #   # alpn: [h2, http/1.1]
  #   # proxy: ""
  #   # rate-limit: 0 # ограничение скорости перенаправления (fallback), в бит/с; 0 означает без ограничений
  ### ВНИМАНИЕ: Для слушателя anytls, если "allow-insecure" не равен true, необходимо заполнить как минимум одну из опций: "certificate и private-key" или "shadow-tls" или "res-tls" или "jls-config" ###
  # allow-insecure: false # Разрешить ли отключение шифрования TLS (Примечание: используется только при предварительной установке nginx или caddy)
  padding-scheme: "" # https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
```

[Общие поля](./index.md)

!!! warning ""
    Если `allow-insecure` не равен `true`, необходимо настроить как минимум один вариант: `certificate` и `private-key`, `shadow-tls`, `res-tls` или `jls-config`.

## padding-scheme

См. https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
