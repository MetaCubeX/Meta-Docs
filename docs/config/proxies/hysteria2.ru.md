# Hysteria2

[Справочная конфигурация](https://hysteria.network/ru/docs/advanced/Full-Client-Config/)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443
  ports: 443-8443
  hop-interval: 30
  password: yourpassword
  up: "30 Mbps"
  down: "200 Mbps"
  obfs: salamander
  obfs-password: yourpassword
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx
  alpn:
    - h3
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## ports

При настройке включает прыжки по портам, игнорирует `port`, формат описан в [диапазонах портов](../../handbook/syntax.md#port-ranges)

## hop-interval

Интервал переключения портов в секундах, по умолчанию 30.

Ввод "15-30" будет случайным образом выбирать одно из значений в качестве интервала переключения каждый раз. Поддерживается только диапазон (запятые не допускаются).

## password

Пароль аутентификации

## up/down

Управление скоростью brutal, если единица измерения не указана, по умолчанию в Mbps

## obfs

Тип обфускатора трафика QUIC. Можно установить `salamander` или `gecko`; если значение пустое, обфускация отключена.

## obfs-password

Пароль для обфускатора трафика QUIC.

## obfs-min-packet-size

Минимальный размер сетевого пакета (в байтах). Доступно только для `gecko`.

## obfs-max-packet-size

Максимальный размер сетевого пакета (в байтах). Доступно только для `gecko`.

## realm-opts
