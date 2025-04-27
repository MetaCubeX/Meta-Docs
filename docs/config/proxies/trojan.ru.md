# Trojan

```{.yaml linenums="1"}
proxies:
- name: "trojan"
  type: trojan
  server: server
  port: 443
  password: yourpsk
  udp: true

  sni: example.com
  alpn:
  - h2
  - http/1.1
  client-fingerprint: random
  fingerprint: xxxx
  skip-cert-verify: true
  ss-opts:
    enabled: false
    method: aes-128-gcm
    password: "example"
  reality-opts:
    public-key: xxxx
    short-id: xxxx

  network: tcp

  smux:
    enabled: false
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## password

Обязательно, пароль сервера trojan

## ss-opts

### ss-opfs.enabled

Включить шифрование AEAD shadowsocks для trojan-go

### ss-opfs.method

Метод шифрования, поддерживает aes-128-gcm/aes-256-gcm/chacha20-ietf-poly1305

### ss-opfs.password

Пароль шифрования AEAD shadowsocks для trojan-go

## network

Транспортный уровень, поддерживает ws/grpc, если не настроен или настроен с другим значением, используется tcp

См. [Конфигурация транспортного уровня](./transport.md) 