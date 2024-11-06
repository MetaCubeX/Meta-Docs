# SOCKS

```{.yaml linenums="1"}
listeners:
- name: socks-in
  type: socks
  port: 7891
  listen: 0.0.0.0
  udp: true
  users:
    - username: username1
      password: password1
```

## [General Fields](./index.md)

## Protocol Configuration

### UDP

Whether to listen for UDP

### User Authentication

If the users field is not filled in, it will follow the global [User Authentication](../../general.md/#user-authentication) settings. If it is filled in, the global settings will be ignored. To bypass authentication for this inbound connection, you can set users: []
