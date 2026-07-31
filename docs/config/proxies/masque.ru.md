# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true

- name: "masque-h3-l4proxy"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  udp: false
  network: h3-l4proxy

- name: "masque-h2"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
  network: h2
```

## Получение конфигурации MASQUE

Сгенерируйте конфигурацию MASQUE с помощью инструмента [usque](https://github.com/Diniboy1123/usque).

[Общие поля](./index.md)

## private-key

Обязательно, закрытый ключ ECDSA в кодировке base64.

## public-key

Обязательно, открытый ключ ECDSA в кодировке base64 (публичный ключ сервера).

!!! note
    Удалите маркеры заголовка/окончания PEM, например `-----BEGIN PUBLIC KEY-----`, `-----END PUBLIC KEY-----`, а также переносы строк PEM `\n`.

## ip

Локальный IPv4-адрес в формате CIDR (например, 172.16.0.2/32).

## ipv6

Локальный IPv6-адрес в формате CIDR (например, fd00::2/128).

## mtu

Размер MTU устройства TUN, по умолчанию 1280.

## udp

Включать ли поддержку UDP, по умолчанию false.

## remote-dns-resolve

Включать ли удаленное разрешение DNS через туннель MASQUE.

## dns

Список удаленных DNS-серверов, действует при включенном `remote-dns-resolve`.

## congestion-controller

Изменяет алгоритм управления перегрузкой по умолчанию. По умолчанию отключено, доступные значения включают `bbr`.

## network

Необязательное поле. По умолчанию используется `quic`. Для `masque-h2` нужно установить `h2`, для режима h3-l4proxy — `h3-l4proxy`.

!!! note
    Режим `h3-l4proxy` сейчас не поддерживает UDP.

## handshake-timeout

Таймаут рукопожатия в секундах. Значение по умолчанию `0` означает, что используется только таймаут внешнего соединения.
