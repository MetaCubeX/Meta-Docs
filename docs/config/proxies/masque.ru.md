# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: "BASE64_ENCODED_PRIVATE_KEY"
  public-key: "BASE64_ENCODED_PUBLIC_KEY"
  # ip: 172.16.0.2/32
  # ipv6: fd00::2/128
  # uri: "/.well-known/masque/udp"
  # sni: server.com
  # mtu: 1280
  # udp: true
  # congestion-controller: bbr
  # cwnd: 10
  # remote-dns-resolve: true
  # dns:
  #   - 8.8.8.8
  #   - 1.1.1.1
```

[Общие поля](./index.md)

## private-key

Обязательно, закрытый ключ ECDSA в кодировке base64

## public-key

Обязательно, открытый ключ ECDSA в кодировке base64 (открытый ключ сервера)

## ip

Локальный IPv4-адрес в формате CIDR (например, 172.16.0.2/32)

## ipv6

Локальный IPv6-адрес в формате CIDR (например, fd00::2/128)

## uri

Путь URI MASQUE, по умолчанию `/.well-known/masque/udp`

## sni

TLS SNI (Server Name Indication), по умолчанию `masque`

## mtu

Размер MTU устройства TUN, по умолчанию 1280

## udp

Включить ли поддержку UDP, по умолчанию false

## congestion-controller

Алгоритм контроля перегрузки, доступные варианты: `cubic`/`new_reno`/`bbr`

## cwnd

Размер окна перегрузки

## remote-dns-resolve

Включить ли удаленное разрешение DNS через туннель MASQUE

## dns

Список удаленных DNS-серверов, действует при включении `remote-dns-resolve`
