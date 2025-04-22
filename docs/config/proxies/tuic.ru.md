# TUIC

```{.yaml linenums="1"}
proxies:
- name: tuic
  server: www.example.com
  port: 10443
  type: tuic
  token: TOKEN
  uuid: 00000000-0000-0000-0000-000000000001
  password: PASSWORD_1
  # ip: 127.0.0.1
  # heartbeat-interval: 10000
  # alpn: [h3]
  disable-sni: true
  reduce-rtt: true
  request-timeout: 8000
  udp-relay-mode: native
  # congestion-controller: bbr
  # max-udp-relay-packet-size: 1500
  # fast-open: true
  # skip-cert-verify: true
  # max-open-streams: 20
  # sni: example.com
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## token

Обязательно, идентификатор пользователя для TUIC V4, не может быть указан при использовании TUIC V5

## uuid

Обязательно, уникальный идентификационный код пользователя для TUIC V5, не может быть указан при использовании TUIC V4

## password

Обязательно, пароль пользователя для TUIC V5, не может быть указан при использовании TUIC V4

## ip

Используется для переопределения результатов DNS-поиска адреса сервера, указанного в опции "server"

## heartbeat-interval

Интервал отправки пакетов активности соединения, в миллисекундах

## disable-sni

Указывает, отключать ли SNI (Server Name Indication) в TLS-рукопожатии. SNI используется для размещения нескольких HTTPS-сайтов на одном IP-адресе

## reduce-rtt

Указывает, включать ли рукопожатие QUIC 0-RTT на клиенте. Это может сократить время установки соединения, но может увеличить риск атак повторного воспроизведения

## request-timeout

Устанавливает время ожидания для установки соединения с прокси-сервером TUIC, в миллисекундах

## udp-relay-mode

Устанавливает режим ретрансляции UDP-пакетов, может быть `native` или `quic`

## congestion-controller

Устанавливает алгоритм контроля перегрузки, доступные варианты: `cubic`/`new_reno`/`bbr`

## max-udp-relay-packet-size

Устанавливает максимальный размер ретранслируемого UDP-пакета, в байтах

## fast-open

Указывает, включать ли Fast Open, что может сократить время установки соединения

## max-open-streams

Устанавливает максимальное количество открытых потоков. Слишком много открытых потоков может повлиять на производительность 