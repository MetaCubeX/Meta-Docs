# Hysteria2

[Справочная конфигурация](https://hysteria.network/zh/docs/advanced-usage/#%e5%ae%a2%e6%88%b7%e7%ab%af)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443
  ports: 443-8443
  password: yourpassword
  up: "30 Mbps"
  down: "200 Mbps"
  obfs: salamander # по умолчанию пусто, если указать - включается obfs, в настоящее время поддерживается только salamander
  obfs-password: yourpassword

  sni: server.com
  skip-cert-verify: false
  fingerprint: xxxx
  alpn:
    - h3
  ###специальные настройки quic-go, не изменяйте без необходимости, если не знаете, что делаете###
  # initial-stream-receive-window： 8388608
  # max-stream-receive-window： 8388608
  # initial-connection-receive-window： 20971520
  # max-connection-receive-window： 20971520
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## ports

При настройке включает прыжки по портам, игнорирует `port`, формат описан в [диапазон портов](../../handbook/syntax.md#_14)

## password

Пароль аутентификации

## up/down

Управление скоростью brutal, если единица измерения не указана, по умолчанию в Mbps

## obfs

Тип маскировки трафика QUIC, можно установить только `salamander`, если пусто, то отключено

## obfs-password

Пароль для маскировки трафика QUIC 
