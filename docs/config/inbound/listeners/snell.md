# Snell

```{.yaml linenums="1"}
listeners:
  - name: snell-in-1
    type: snell
    port: 10815 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
    listen: 0.0.0.0
    # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
    psk: your-password
    version: 4 # 仅支持 4/5
    udp: true # UDP over TCP tunnel，默认 true
    # obfs-opts:
    #   mode: http # 可选：http / tls
    #   host: bing.com
    # shadow-tls:
    #   enable: false # 设置为true时开启
    #   version: 3 # 支持v1/v2/v3
    #   password: password # v2设置项
    #   users: # v3设置项
    #     - name: 1
    #       password: password
    #   handshake:
    #     dest: test.com:443
    # rule: sub-rule-name1
    # proxy: proxy
```
