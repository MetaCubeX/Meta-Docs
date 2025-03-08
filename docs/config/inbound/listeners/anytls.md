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

[通用字段](./index.md)

!!! warning ""
    证书和私钥是必要的

## padding-scheme

参阅 https://github.com/anytls/anytls-go/blob/main/docs/protocol.md#cmdupdatepaddingscheme