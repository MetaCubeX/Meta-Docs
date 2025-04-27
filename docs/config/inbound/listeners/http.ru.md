# HTTP

```{.yaml linenums="1"}
listeners:
- name: http-in
  type: http
  port: 7890
  listen: 0.0.0.0
  users:
    - username: username1
      password: password1
  certificate: ./server.crt
  private-key: ./server.key
```

## [Общие поля](./index.md)

## Настройка протокола

### Аутентификация пользователей

Если не заполнено поле users, то используются глобальные настройки [аутентификации пользователей](../../general.md/#_2). Если заполнено, глобальные настройки игнорируются. Чтобы пропустить аутентификацию для этого входящего соединения, можно указать `users: []` 