# Mieru

```{.yaml linenums="1"}
proxies:
  - name: mieru
    type: mieru
    server: server
    port: 2999
    port-range: 2090-2099
    transport: TCP
    username: user
    password: password
    multiplexing: MULTIPLEXING_LOW
    traffic-pattern: ""
```

[通用字段](./index.md)

## port-range

端口范围，不可与 `port` 同时书写

## transport

协议，目前支持 `TCP` 和 `UDP`

## multiplexing

多路复用，可以使用的值包括 `MULTIPLEXING_OFF`, `MULTIPLEXING_LOW`, `MULTIPLEXING_MIDDLE`, `MULTIPLEXING_HIGH`。其中 `MULTIPLEXING_OFF` 会关闭多路复用功能。默认值为 `MULTIPLEXING_LOW`

## traffic-pattern

一个 base64 字符串用于微调网络行为, 格式请参考[mieru官方文档](https://github.com/enfein/mieru/blob/main/docs/traffic-pattern.md)
