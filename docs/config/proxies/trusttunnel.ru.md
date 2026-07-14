# TrustTunnel

```yaml
proxies:
- name: trusttunnel
  type: trusttunnel
  server: 1.2.3.4
  port: 443
  username: username
  password: password
  # client-fingerprint: chrome
  health-check: true
  udp: true
  # sni: "example.com"
  # alpn:
  #   - h2
  # skip-cert-verify: true
  ### параметры QUIC
  # quic: true
  # congestion-controller: bbr
  # bbr-profile: "" # Доступные значения: "standard", "conservative", "aggressive". По умолчанию: "standard"
  ### параметры повторного использования соединений
  # max-connections: 8
  # min-streams: 5
  # max-streams: 0
```

[Общие поля](./index.md)

[Поля TLS](./tls.md)

## quic

Включает использование QUIC. По умолчанию: `false`.

## congestion-controller

Задает алгоритм управления перегрузкой (congestion control) для QUIC.

### max-connections

Максимальное количество соединений.

Несовместим с параметром `max-streams`.

### min-streams

Минимальное количество мультиплексируемых потоков в одном соединении, после которого будет открыто новое соединение.

Несовместим с параметром `max-streams`.

### max-streams

Максимальное количество мультиплексируемых потоков в одном соединении, после которого будет открыто новое соединение.

Несовместим с параметрами `max-connections` и `min-streams`.
