# MIXED

```{.yaml linenums="1"}
listeners:
- name: mixed-in
  type: mixed
  port: 7892
  listen: 0.0.0.0
  udp: true
  users:
    - username: username1
      password: password1
  # следующие два параметра, если заполнены, включают tls (необходимо заполнить оба)
  # certificate: ./server.crt # сертификат в формате PEM или путь к сертификату
  # private-key: ./server.key # приватный ключ сертификата в формате PEM или путь к приватному ключу
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
```

## [Общие поля](./index.md)

## Настройка протокола

### udp

Прослушивать ли UDP

### Аутентификация пользователей

Если не заполнено поле users, то используются глобальные настройки [аутентификации пользователей](../../general.md/#_2). Если заполнено, глобальные настройки игнорируются. Чтобы пропустить аутентификацию для этого входящего соединения, можно указать users: [] 