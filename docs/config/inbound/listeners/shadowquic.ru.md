# ShadowQUIC

```{.yaml linenums="1"}
listeners:
- name: shadowquic-in-1
  type: shadowquic
  port: 10822
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для слушающего сокета (только Linux)
  # rule: sub-rule-name1
  # proxy: proxy
  users:
    - username: username
      password: password
  jls-upstream:
    addr: www.example.com:443
    # sni: example.com
    # proxy: proxy
    # rate-limit: 0
    # quic-version-probe: false
  # alpn:
  #   - h3
  # quic-versions: [v1]
  # zero-rtt: true
  # congestion-controller: bbr
  # up: 100 Mbps
  # down: 100 Mbps
  # ignore-client-bandwidth: false
  # cwnd: 10
  # bbr-profile: ""
  # max-idle-time: 30000
  # max-datagram-frame-size: 1400
  # recv-window-conn: 0
  # recv-window: 0
  # disable-mtu-discovery: false
```

[Общие поля](./index.md)

## users

Пользователи аутентификации ShadowQUIC.

### users.username

Имя пользователя.

### users.password

Пароль.

## jls-upstream

Соединения, не прошедшие JLS-аутентификацию, перенаправляются на этот маскирующий QUIC upstream.

### jls-upstream.addr

Адрес маскирующего upstream.

### jls-upstream.sni

Целевой SNI для JLS-маскировки. Если пусто, выводится из `addr`.

### jls-upstream.proxy

Прокси для маскирующего upstream.

### jls-upstream.rate-limit

Ограничение скорости пересылки в bit/s. `0` означает без ограничения.

### jls-upstream.quic-version-probe

Проверять версию QUIC маскирующего upstream при первом соединении.

## alpn

Список согласования протоколов прикладного уровня.

## quic-versions

Локально заданная версия и fallback при ошибке проверки. Поддерживает `v1` / `v2`.

## zero-rtt

Включить ли 0-RTT.

## congestion-controller

Алгоритм управления перегрузкой. Возможные значения: `cubic` / `new_reno` / `bbr`. По умолчанию `cubic`.

## up/down

Согласование пропускной способности Brutal. `up` задает лимит server upload / client download, `down` задает запрашиваемую скорость server download / client upload.

## ignore-client-bandwidth

Для Brutal. Игнорирует outbound `down` и использует listener `down` или автоматическую настройку.

## cwnd

Начальный размер окна перегрузки. По умолчанию `32`.

## bbr-profile

Профиль агрессивности BBR. Возможные значения: `standard` / `conservative` / `aggressive`. По умолчанию `standard`.

## max-idle-time

Максимальное время простоя в миллисекундах.

## max-datagram-frame-size

Максимальный размер UDP datagram frame.

## recv-window-conn

Окно приема на уровне соединения. `0` использует значение по умолчанию.

## recv-window

Окно приема на уровне потока. `0` использует значение по умолчанию.

## disable-mtu-discovery

Отключить ли path MTU discovery.
