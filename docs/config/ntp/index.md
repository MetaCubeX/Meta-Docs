# NTP

```{.yaml linenums="1"}
ntp:
  enable: true
  write-to-system: true
  server: time.apple.com
  port: 123
  interval: 30
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
