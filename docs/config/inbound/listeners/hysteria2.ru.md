# Hysteria2

```{.yaml linenums="1"}
listeners:
- name: hy-in
  type: hysteria2
  port: 8443
  listen: 0.0.0.0
  users:
    user1: password1
    user2: password2
  up: 1000
  down: 1000
  ignore-client-bandwidth: false
  obfs: salamander
  obfs-password: password
  masquerade: ""
  alpn:
  - h3
  certificate: ./server.crt # сертификат в формате PEM или путь к сертификату
  private-key: ./server.key # приватный ключ сертификата в формате PEM или путь к приватному ключу
  # следующие два параметра для конфигурации mTLS, если client-auth-type установлен в "verify-if-given" или "require-and-verify", то client-auth-cert не должен быть пустым
  # client-auth-type: "" # возможные значения: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # сертификат в формате PEM или путь к сертификату
  # если заполнен, то включается ech (можно сгенерировать с помощью mihomo generate ech-keypair <доменное имя в открытом виде>)
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
```

## [Общие поля](./index.md)

## Настройка протокола

### users

Пользователи Hysteria и пароли для аутентификации, формат: `имя_пользователя: пароль`

!!! note ""
    Имя пользователя не участвует в аутентификации, используется только для сопоставления с [правилами входящих соединений](../../rules/index.md#in-user)

### up/down

Настройки скорости Hysteria, по умолчанию в Mbps, подробнее см. [документацию Hysteria](https://v2.hysteria.network/zh/docs/advanced/Full-Server-Config/#_4)

### ignore-client-bandwidth

Возможные значения: `true/false`

При включении сервер будет игнорировать любые настройки пропускной способности клиента и всегда использовать традиционный алгоритм контроля перегрузки (BBR)

### obfs

Маскировка трафика QUIC, можно установить только `salamander`, если пусто, то отключено

!!! warning ""
    Включение маскировки сделает сервер несовместимым со стандартными QUIC-соединениями, что приведет к потере возможности маскироваться под HTTP/3

### obfs-password

Пароль для маскировки трафика QUIC

### masquerade

Маскировка трафика под HTTP/3, поддерживает только `file` и `http/https`, если пусто, всегда возвращает 404 Not Found

Подробнее см. [документацию Hysteria](https://v2.hysteria.network/zh/docs/advanced/Full-Server-Config/#masquerade)

|              | Пример                   | Описание       |
|--------------|-------------------------|----------------|
| `file`       | `file:///var/www`       | Файловый сервер |
| `http/https` | `http://127.0.0.1:8080` | Обратный прокси |

### alpn

Список протоколов согласования прикладного уровня TLS, клиент должен указать один из протоколов из списка сервера, иначе аутентификация не пройдет. По умолчанию `h3`

### certificate/private-key

Пути к файлам сертификата TLS 