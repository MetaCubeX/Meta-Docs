# AnyTLS

```{.yaml linenums="1"}
listeners:
- name: anytls-in-1
  type: anytls
  port: 10818
  listen: 0.0.0.0
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
  padding-scheme: "" # https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
```

[Общие поля](./index.md)

!!! warning ""
    `certificate` и `private-key` обязательны

## padding-scheme

См. https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme 