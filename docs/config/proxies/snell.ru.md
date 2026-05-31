# Snell

```{.yaml linenums="1"}
proxies
- name: "snell"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # udp: true
  # reuse: false
  # obfs-opts:
  #   mode: http
  #   host: bing.com

```

[Общие поля](./index.md)

## psk

Обязательно, предварительный общий ключ Snell

## version

Версия Snell. Поддерживаются v1/2/3/4/5. UDP поддерживается только в версиях v3/4/5.

## reuse

Необязательный. Поддерживается в v4/5. По умолчанию false.

## obfs-opts

Настройки маскировки Snell

### obfs-opts.mode

Режим маскировки Snell, поддерживает http/tls

### obfs-opts.host

Домен для маскировки Snell 
