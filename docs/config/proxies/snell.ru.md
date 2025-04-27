# Snell

```{.yaml linenums="1"}
proxies
- name: "snell"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  version: 3
  obfs-opts:
    mode: http
    host: bing.com
```

[Общие поля](./index.md)

## psk

Обязательно, предварительный общий ключ Snell

## version

Версия snell, поддерживаются только v1-3, по умолчанию v1, только v3 поддерживает udp

## obfs-opts

Настройки маскировки Snell

### obfs-opts.mode

Режим маскировки Snell, поддерживает http/tls

### obfs-opts.host

Домен для маскировки Snell 