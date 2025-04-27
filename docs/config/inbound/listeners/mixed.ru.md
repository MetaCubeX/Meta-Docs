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
  certificate: ./server.crt
  private-key: ./server.key
```

## [Общие поля](./index.md)

## Настройка протокола

### udp

Прослушивать ли UDP

### Аутентификация пользователей

Если не заполнено поле users, то используются глобальные настройки [аутентификации пользователей](../../general.md/#_2). Если заполнено, глобальные настройки игнорируются. Чтобы пропустить аутентификацию для этого входящего соединения, можно указать users: [] 