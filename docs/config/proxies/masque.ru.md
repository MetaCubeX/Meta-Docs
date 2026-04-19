# MASQUE

```{.yaml linenums="1"}
proxies:
# masque
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
  # Идентификатор исходящего прокси. Если значение не пустое, соединения будут отправляться через указанный proxy
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Принудительное удаленное DNS-разрешение, значение по умолчанию — false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Работает только при remote-dns-resolve: true
  # congestion-controller: bbr # По умолчанию отключено

# masque-h2
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
  # Идентификатор исходящего прокси. Если значение не пустое, соединения будут отправляться через указанный proxy
  # dialer-proxy: "ss1"
  # remote-dns-resolve: true # Принудительное удаленное DNS-разрешение, значение по умолчанию — false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Работает только при remote-dns-resolve: true
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

Необязательное поле. По умолчанию используется `quic`, для `masque-h2` нужно установить `h2`.
