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
  certificate: ./server.crt
  private-key: ./server.key
  padding-scheme: "" # https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme
```

[Общие поля](./index.md)

!!! warning ""
    `certificate` и `private-key` обязательны

## padding-scheme

См. https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme 