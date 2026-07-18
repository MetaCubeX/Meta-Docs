# Mieru

```{.yaml linenums="1"}
listeners:
- name: mieru-in-1
  type: mieru
  port: 10818
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  transport: TCP # Supports TCP or UDP
  users:
    username1: password1
    username2: password2
  # A base64 string used to fine-tune network behavior
  # traffic-pattern: ""
  # When enabled, the proxy server rejects clients that do not send a user hint
  # user-hint-is-mandatory: false
```

[General Fields](./index.md)
