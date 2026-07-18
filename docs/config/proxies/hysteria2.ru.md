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
  # bbr-profile: "" # Возможные значения: "standard", "conservative", "aggressive". По умолчанию: "standard"
  obfs: salamander # Тип обфускатора трафика QUIC. Можно установить значение `salamander` или `gecko`. Если оставить пустым, обфускация будет отключена.
  obfs-password: yourpassword
  # obfs-min-packet-size: 512
  # obfs-max-packet-size: 1200
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx # Настройка отпечатка обеспечивает SSL pinning. Получить его можно командой: openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
  alpn:
    - h3
  # realm-opts:
  #   enable: true # Необходимо включить вручную.
  #   server-url: https://realm.hy2.io
  #   token: public
  #   realm-id: my-cabin-1f3a8c2e9b
  #   stun-servers:
  #     - stun.nextcloud.com:3478
  #     - stun.sip.us:3478
  #     - global.stun.twilio.com:3478
  #   # Следующая инструкция позволяет ввести конфигурацию TLS для URL-адреса сервера (sni, skip-cert-verify, name-cert-verify, fingerprint, certificate, private-key, alpn)
  #   # skip-cert-verify： false
  #   # name-cert-verify: example.com
  #   # ......
  ###специальные настройки quic-go, не изменяйте без необходимости, если не знаете, что делаете###
  # initial-stream-receive-window： 8388608
  # max-stream-receive-window： 8388608
  # initial-connection-receive-window： 20971520
  # max-connection-receive-window： 20971520
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
