# VMess

```{.yaml linenums="1"}
proxies:
- name: "vmess"
  type: vmess
  server: server
  port: 443
  udp: true
  uuid: uuid
  alterId: 0
  cipher: auto
  packet-encoding: packetaddr
  global-padding: false
  authenticated-length: false

  tls: true
  servername: example.com
  alpn:
  - h2
  - http/1.1
  fingerprint: xxxx
  client-fingerprint: chrome
  skip-cert-verify: true
  name-cert-verify: example.com
  shadow-tls-opts:
    version: 3
    password: shadow-tls-password
  restls-opts:
    password: restls-password
    version-hint: tls13
  jls-opts:
    username: jls-user
    password: jls-password
  reality-opts:
    public-key: xxxx
    short-id: xxxx
  tlsmirror-opts:
    primary-key: xxxx

  network: tcp

  smux:
    enabled: false
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## uuid

Обязательно, ID пользователя VMess

## alterId

Обязательно, если не 0, активирует старый протокол

## cipher

Обязательно, метод шифрования, поддерживает `auto`/`none`/`zero`/`aes-128-gcm`/`chacha20-poly1305`

## packet-encoding

Кодирование UDP-пакетов, если пусто, используется оригинальное кодирование. Возможные значения: `packetaddr` (поддерживается `v2ray 5+`) / `xudp` (поддерживается `xray`)

## global-padding

Параметр протокола. Если включен, случайным образом расходует трафик (в v2ray включен по умолчанию и не может быть отключен).

## authenticated-length

Параметр протокола. Включает шифрование блоков длины

## network

Транспортный уровень, поддерживает ws/http/h2/grpc/mkcp/mekya. Если не настроен или настроен с другим значением, используется tcp

См. [Конфигурация транспортного уровня](./transport.md)
