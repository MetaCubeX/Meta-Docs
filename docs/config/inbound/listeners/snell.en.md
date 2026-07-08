# Snell

```{.yaml linenums="1"}
listeners:
  - name: snell-in-1
    type: snell
    port: 10815 # Supports ports format, e.g. 200,302 or 200,204,401-429,501-503
    listen: 0.0.0.0
    # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
    psk: your-password
    version: 4 # Supports 1/2/3/4/5
    udp: true # UDP over TCP tunnel, defaults to true
    # obfs-opts:
    #   mode: http # Optional: http / tls
    #   host: bing.com
    # shadow-tls:
    #   enable: false # Set to true to enable
    #   version: 3 # Supports v1/v2/v3
    #   password: password # v2 option
    #   users: # v3 option
    #     - name: 1
    #       password: password
    #   handshake:
    #     dest: test.com:443
    # rule: sub-rule-name1
    # proxy: proxy
```

