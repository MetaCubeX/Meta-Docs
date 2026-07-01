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

- name: "snell-shadow-tls"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  # version: 4
  # udp: true
  # reuse: false
  # client-fingerprint: chrome
  obfs-opts:
     mode: shadow-tls
     host: bing.com
     password: "shadow_tls_password"
     version: 2
     alpn: ["h2"]

```

[Общие поля](./index.md)

## psk

Обязательно. Предварительно общий ключ (PSK) Snell.

## version

Версия Snell. Поддерживаются v1/2/3/4/5. Только v3/4/5 поддерживают UDP.

## reuse

Необязательно. Поддерживается в v4/5. По умолчанию: false.

## client-fingerprint

Необязательно. По умолчанию: chrome.

## obfs-opts

Настройки обфускации Snell.

### obfs-opts.mode

Режим обфускации Snell. Поддерживаются http/tls/shadow-tls.

### obfs-opts.host

Домен для обфускации Snell.

### obfs-opts.password

Пароль shadow-tls. Требуется только при использовании shadow-tls.

### obfs-opts.version

Версия shadow-tls. Поддерживаются v1/2/3. Требуется только при использовании shadow-tls.

### obfs-opts.alpn

Поддерживаются h2 и http1.1. Требуется только при использовании shadow-tls.
