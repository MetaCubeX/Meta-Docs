# Конфигурация TLS

```{.yaml linenums="1"}
proxies:
- name: "tls-example"
  tls: true
  sni: example.com
  servername: example.com
  fingerprint: xxx
  alpn:
  - h2
  - http/1.1
  skip-cert-verify: true
  client-fingerprint: random
  reality-opts:
    public-key: xxxx
    short-id: xxxx
```

## tls

Включить tls, применяется только к протоколам, использующим `tls`, протокол `trojan` принудительно включает

## sni/servername

Server Name Indication, в [`VMess`](./vmess.md)/[`VLESS`](./vless.md) - `servername`, если пусто, то используется адрес из поля `server`

## fingerprint

Отпечаток сертификата, применяется только к протоколам, использующим `tls`, можно получить с помощью 
```bash
openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
```

## alpn

Список поддерживаемых протоколов согласования прикладного уровня, расположенных в порядке приоритета.

Если обе стороны поддерживают ALPN, выбранный протокол будет одним из этого списка, если нет взаимно поддерживаемых протоколов, соединение не будет установлено.

См. [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

Пропустить проверку сертификата, применяется только к протоколам, использующим `tls`

## client-fingerprint

Отпечаток клиента utls, применяется только к протоколам [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md), возможные значения см. [глобальный отпечаток клиента](../general.md#_14)

## reality-opts

Настройки reality, если не пусто, включает reality

### reality-opts.public-key

Публичный ключ, соответствующий приватному ключу сервера reality

### reality-opts.short-id

Один из short id сервера 