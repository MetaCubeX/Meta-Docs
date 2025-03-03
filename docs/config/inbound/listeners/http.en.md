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

## [General Fields](./index.md)

## Protocol Configuration

### User Authentication

If the users field is not filled in, it will follow the global [User Authentication](../../general.md/#user-authentication) settings. If it is filled in, the global settings will be ignored. To skip authentication for this inbound connection, you can set `users: []`.
