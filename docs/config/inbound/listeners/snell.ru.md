# Snell

```{.yaml linenums="1"}
listeners:
  - name: snell-in-1
    type: snell
    port: 10815 # Поддерживается формат ports, например: 200,302 или 200,204,401-429,501-503
    listen: 0.0.0.0
    # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
    psk: your-password
    version: 4 # Поддерживаются только 4/5
    udp: true # UDP через TCP-туннель, по умолчанию true
    # obfs-opts:
    #   mode: http # Необязательно: http / tls
    #   host: bing.com
    # shadow-tls:
    #   enable: false # Установите true для включения
    #   version: 3 # Поддерживаются v1/v2/v3
    #   password: password # Параметр для v2
    #   users: # Параметр для v3
    #     - name: 1
    #       password: password
    #   handshake:
    #     dest: test.com:443
    # rule: sub-rule-name1
    # proxy: proxy
```

