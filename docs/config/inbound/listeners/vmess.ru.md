# VMESS

```{.yaml linenums="1"}
listeners:
- name: vmess-in-1
  type: vmess
  port: 10814 # поддерживает формат ports, например 200,302 или 200,204,401-429,501-503
  listen: 0.0.0.0
  # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
  # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
  users:
    - username: 1
      uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
      alterId: 1
  # ws-path: "/" # если не пусто, то включается транспортный уровень websocket
  # grpc-service-name: "GunService" # если не пусто, то включается транспортный уровень grpc
  # следующие два параметра, если заполнены, включают tls (необходимо заполнить оба)
  # certificate: ./server.crt # сертификат в формате PEM или путь к сертификату
  # private-key: ./server.key # приватный ключ сертификата в формате PEM или путь к приватному ключу
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
  # если заполнен reality-config, то включается reality (не может использоваться одновременно с certificate и private-key)
  # reality-config:
  #   dest: test.com:443
  #   private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # можно сгенерировать командой mihomo generate reality-keypair
  #   short-id:
  #     - 0123456789abcdef
  #   server-names:
  #     - test.com
  #   #следующие два limit необязательны, могут ограничивать скорость fallback-соединений, не прошедших проверку, bytesPerSec по умолчанию 0 (не включено)
  #   #ограничение скорости fallback является характеристикой, не рекомендуется включать, если вы разработчик панели/скрипта, обязательно рандомизируйте эти параметры
  #   limit-fallback-upload:
  #     after-bytes: 0 # начать ограничение скорости после передачи указанного количества байт
  #     bytes-per-sec: 0 # базовая скорость (байт/сек)
  #     burst-bytes-per-sec: 0 # пиковая скорость (байт/сек), действует когда больше bytesPerSec
  #   limit-fallback-download:
  #     after-bytes: 0 # начать ограничение скорости после передачи указанного количества байт
  #     bytes-per-sec: 0 # базовая скорость (байт/сек)
  #     burst-bytes-per-sec: 0 # пиковая скорость (байт/сек), действует когда больше bytesPerSec
``` 