# NTP

```{.yaml linenums="1"}
ntp:
  enable: true
  write-to-system: true
  server: time.apple.com
  port: 123
  interval: 30
  # dialer-proxy: DIRECT
```

## enable

是否启用 NTP 服务

## write-to-system

是否同步至系统时间，需要 root、管理员模式运行。

## server

NTP 服务地址，默认 `time.apple.com`

## port

NTP 服务端口，默认 `123`

## interval

同步时间间隔，单位（分），默认同步间隔为 30 分

## dialer-proxy

可选，默认为`DIRECT`

!!! note
    NTP默认直接对目标发起连接，不经过出站和路由规则，可以用dialer-proxy来强制指定
