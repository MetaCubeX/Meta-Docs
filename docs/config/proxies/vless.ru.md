# VLESS

```{.yaml linenums="1"}
proxies:
- name: "vless"
  type: vless
  server: server
  port: 443
  udp: true
  uuid: uuid
  flow: xtls-rprx-vision
  packet-encoding: xudp

  tls: true
  servername: example.com
  alpn:
  - h2
  - http/1.1
  fingerprint: xxxx
  client-fingerprint: chrome
  skip-cert-verify: true
  reality-opts:
    public-key: xxxx
    short-id: xxxx
  encryption: ""

  network: tcp

  smux:
    enabled: false
```

!!! note
    Контроль потока `xtls-*` в Meta фактически эквивалентен `xtls-*-udp443` в Xray-core. Если нужно перехватывать UDP-трафик на порту 443, используйте логическое правило: `AND,((NETWORK,UDP),(DST-PORT,443)),REJECT`

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## uuid

Обязательно, ID пользователя VLESS

## flow

Подпротокол VLESS, доступное значение - `xtls-rprx-vision`

## packet-encoding

Кодирование UDP-пакетов, если пусто, используется оригинальное кодирование. Возможные значения: `packetaddr` (поддерживается `v2ray 5+`) / `xudp` (поддерживается `xray`)

## encryption

Конфигурация клиента шифрования Vless:

`encryption: "mlkem768x25519plus.native/xorpub/random.1rtt/0rtt.(Пароль X25519).(Клиент ML-KEM-768)..."`

(XTLS Vision в native/xorpub можно разделить. Используется только режим 1-RTT. / Если секунды в тикете, отправленном сервером, не равны нулю, используется мультиплексирование 0-RTT.)

/ Можно выбрать только один параметр, за которым следует как минимум одна строка base64. Неограниченное количество конкатенаций. Для генерации используйте `mihomo generate vless-x25519` и `mihomo generate vless-mlkem768`. При замене значения скобки следует удалить.

## network

Транспортный уровень, поддерживает ws/http/h2/grpc, если не настроен или настроен с другим значением, используется tcp

См. [Конфигурация транспортного уровня](./transport.md) 
