# AnyTLS

```{.yaml linenums="1"}
proxies:
- name: anytls
  type: anytls
  server: 1.2.3.4
  port: 443
  password: "<your password>"
  client-fingerprint: chrome
  udp: true
  idle-session-check-interval: 30
  idle-session-timeout: 30
  min-idle-session: 0
  sni: "example.com"
  alpn:
    - h2
    - http/1.1
  skip-cert-verify: true
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

!!! tip
    Mihomo не поддерживает комбинацию AnyTLS+Reality (и не будет поддерживать её в будущем). Если вы хотите скрыть SNI, используйте его в сочетании с [ECH](./tls.md#ech-opts). Если вы настаиваете на использовании Reality, выберите протоколы [Vmess](./vmess.md), [VLESS](./vless.md) или [Trojan](./trojan.md).

## idle-session-check-interval

Интервал проверки неактивных сессий. По умолчанию: 30 секунд.

## idle-session-timeout

При проверке закрывает сессии, неактивные дольше этого значения. По умолчанию: 30 секунд.

## min-idle-session

При проверке первые n неактивных сессий остаются открытыми. По умолчанию: n=0 
