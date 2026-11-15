# Snell

```{.yaml linenums="1"}
listeners:
  - name: snell-in-1
    type: snell
    port: 10815 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
    listen: 0.0.0.0
    # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
    psk: your-password
    version: 4 # 支持 1/2/3/4/5/6
    udp: true # UDP over TCP tunnel，默认 true
    # mode: default # 仅 v6：default / unshaped / unsafe-raw，需与客户端一致；v6 的 psk 长度须为 12 到 255 字节
    # obfs-opts:
    #   mode: http # 可选：http / tls（v6 不支持，配置时会报错）
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
    # res-tls:
    #   enable: false
    #   dest: test.com:443
    #   password: restls-password
    #   rate-limit: 0 # fallback 双向转发限速，单位 bit/s；0 表示不限速
    # jls-config:
    #   enable: false
    #   users:
    #     - username: jls-user
    #       password: jls-password
    #   dest: test.com:443
    ## shadow-tls, res-tls and jls-config are mutually exclusive
    # rule: sub-rule-name1
    # proxy: proxy
```
