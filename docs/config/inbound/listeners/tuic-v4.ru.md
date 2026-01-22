# TUIC V4

```{.yaml linenums="1"}
listeners:
- name: tuicv4-in
  type: tuic
  port: 10003
  listen: 0.0.0.0
  token:
    - TOKEN
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
  congestion-controller: bbr
  max-idle-time: 15000
  authentication-timeout: 1000
  alpn:
    - h3
  max-udp-relay-packet-size: 1500
``` 