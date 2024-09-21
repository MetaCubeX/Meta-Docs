# MIXED

```{.yaml linenums="1"}
listeners:
- name: mixed-in
  type: mixed
  port: 7892
  listen: 0.0.0.0
  udp: true
  users:
    - username: password
```

## [General Fields](./index.md)

## Protocol Configuration

### UDP

Whether to listen for UDP

### User Authentication

If the users field is left empty, it will follow the global [User Authentication](../../general.md/#user-authentication) settings. If filled in, it will override the global settings. To skip authentication for this inbound connection, you can specify users: []