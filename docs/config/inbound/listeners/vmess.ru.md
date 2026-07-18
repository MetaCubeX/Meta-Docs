# VMESS

```{.yaml linenums="1"}
listeners:
- name: vmess-in-1
  type: vmess
  port: 10814 # поддерживает формат ports, например 200,302 или 200,204,401-429,501-503
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
  # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
  # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
  users:
    - username: 1
      uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
      alterId: 1
  # ws-path: "/" # если не пусто, то включается транспортный уровень websocket
  # grpc-service-name: "GunService" # если не пусто, то включается транспортный уровень grpc
  # если заполнен mekya-config и enable: true, включается v2ray-совместимый Mekya inbound (нельзя использовать вместе с mkcp/ws/grpc)
  # mekya-config:
  #   enable: true
  #   max-write-size: 10485760 # максимальный размер полезной нагрузки одного ответа, байт
  #   max-write-duration-ms: 5000 # максимальная длительность одного ответа, миллисекунды
  #   max-simultaneous-write-connection: 128 # число запросов одной сессии, одновременно ожидающих запись
  #   packet-writing-buffer: 65536 # размер буфера записи пакетов
  #   kcp:
  #     mtu: 1350
  #     tti: 15
  #     uplink-capacity: 40
  #     downlink-capacity: 2000
  #     congestion: false
  #     write-buffer: 67108864
  #     read-buffer: 67108864
  #     seed: ""
  #     header: ""
  # если заполнен mkcp-config и enable: true, включается v2ray-совместимый mKCP inbound
  # mkcp-config:
  #   enable: true
  #   mtu: 1350
  #   tti: 50
  #   uplink-capacity: 5
  #   downlink-capacity: 20
  #   congestion: false
  #   write-buffer: 2097152
  #   read-buffer: 2097152
  #   seed: ""
  #   header: ""
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
  # shadow-tls:
  #   enable: true
  #   version: 3 # поддерживает v1/v2/v3
  #   # password: shadow-tls-password # параметр конфигурации v2
  #   users: # параметр конфигурации v3
  #     - name: shadow-tls-user
  #       password: shadow-tls-password
  #   handshake:
  #     dest: www.example.com:443
  #     # proxy: ""
  # jls-config: # JLS заменяет обычный TLS; неавторизованные соединения перенаправляются на dest
  #   enable: true
  #   users:
  #     - username: jls-user
  #       password: jls-password
  #   dest: www.example.com:443
  #   # sni: www.example.com # Если пусто, выводится из dest
  #   # alpn: [h2, http/1.1]
  #   # proxy: ""
  #   # rate-limit: 0 # Ограничение скорости fallback-пересылки, bit/s; 0 означает без ограничения
  # res-tls:
  #   enable: true
  #   dest: www.example.com:443
  #   password: restls-password
  #   # restls-script: ""
  #   # min-record-len: 0
  #   # proxy: ""
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
  # если заполнен tlsmirror-config, включается tlsmirror (нельзя использовать одновременно с certificate и private-key)
  # tlsmirror-config:
  #   dest: test.com:443
  #   primary-key: MDEyMzQ1Njc4OWFiY2RlZjAxMjM0NTY3ODlhYmNkZWY=
  #   proxy: ""
  #   explicit-nonce-ciphersuites: [156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 49195, 49196, 49197, 49198, 49199, 49200, 49201, 49202, 49290, 49291, 49293, 49316, 49317, 49318, 49319, 49320, 49321, 49322, 49323, 49324, 49325, 49326, 49327, 52392, 52393, 52394, 52395, 52396, 52397, 52398]
  #   defer-instance-derived-write-time: # задержка перед первой записью
  #     base-nanoseconds: 0 # фиксированная задержка, наносекунды
  #     uniform-random-multiplier-nanoseconds: 0 # верхняя граница дополнительной случайной задержки, наносекунды
  #   transport-layer-padding: # включить заполнение транспортного уровня
  #     enabled: false
  #   connection-enrolment: # включить v2ray-совместимое подтверждение регистрации соединения
  #     primary-ingress-outbound: "" # v2ray-совместимое поле, рекомендуется согласовать с control outbound tag на другой стороне
  #   sequence-watermarking-enabled: false # включить sequence watermarking
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # по умолчанию в Mbps
  #     down: 1000
```
