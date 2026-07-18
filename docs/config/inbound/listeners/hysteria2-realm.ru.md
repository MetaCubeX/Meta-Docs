# Hysteria2 Realm

```{.yaml linenums="1"}
listeners:
# Это HTTP/HTTPS-сервер для server-url в realm-opts самостоятельно размещенных входящих и исходящих соединений Hysteria2. Не путайте его с сервером Hysteria2.
- name: hysteria2-realm-in-1
  type: hysteria2-realm
  port: 10820 # Поддерживает формат ports, например: 200,302 или 200,204,401-429,501-503
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (только Linux)
  token: public # Bearer-токен, передаваемый входящими и исходящими соединениями Hysteria2 через Authorization: Bearer <token>
  max-realms: 65536 # Максимальное общее количество realms (0 = без ограничений)
  max-realms-per-ip: 4 # Максимальное количество realms на IP-адрес клиента (0 = без ограничений)
  trusted-proxy-header: "" # Заголовок для получения реального IP-адреса клиента, например X-Forwarded-For
  realm-name-pattern: "^[A-Za-z0-9][A-Za-z0-9_-]{0,63}$" # Регулярное выражение, которому должны соответствовать имена realms
  # Если параметры ниже настроены, realm предоставляет HTTPS через TLS; иначе используется незашифрованный HTTP
  # certificate: ./server.crt # Сертификат в формате PEM или путь к сертификату
  # private-key: ./server.key # Соответствующий закрытый ключ в формате PEM или путь к нему
  # Следующие два параметра настраивают mTLS. client-auth-cert не должен быть пустым, если client-auth-type равен "verify-if-given" или "require-and-verify"
  # client-auth-type: "" # Возможные значения: "", "request", "require-any", "verify-if-given", "require-and-verify"
  # client-auth-cert: string # Сертификат в формате PEM или путь к сертификату
  # Если заполнено, включается ECH. Ключ можно создать командой mihomo generate ech-keypair <доменное-имя>
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # alpn: ["h2", "http/1.1"]
```

## [Общие поля](./index.md)

## Настройка протокола
