# VLESS

```{.yaml linenums="1"}
listeners:
- name: vless-in-1
  type: vless
  port: 10817 # поддерживает формат ports, например 200,302 или 200,204,401-429,501-503
  listen: 0.0.0.0
  # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
  # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
  users:
    - username: 1
      uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
      flow: xtls-rprx-vision
  # ws-path: "/" # если не пусто, то включается транспортный уровень websocket
  # grpc-service-name: "GunService" # если не пусто, то включается транспортный уровень grpc
  # -------------------------
  # Конфигурация vless encryption на стороне сервера:
  # (нативный вид / только XOR публичного ключа / полностью случайный. 1-RTT каждый раз выдает случайный ticket на 300-600 секунд для повторного использования 0-RTT / разрешен только 1-RTT)
  # Заполнение "600s" будет случайным образом брать от 50% до 100%, т.е. эквивалентно заполнению "300-600s"
  # / означает выбор только одного варианта, после него base64 - минимум один, бесконечная цепочка, используйте mihomo generate vless-x25519 и mihomo generate vless-mlkem768 для генерации, при замене значения нужно убрать скобки
  #
  # Padding - это необязательный параметр, действует только на 1-RTT для устранения характеристик длины рукопожатия, значение по умолчанию на обеих сторонах "100-111-1111.75-0-111.50-0-3333":
  # После 1-RTT client/server hello с вероятностью 100% добавляется случайный padding от 111 до 1111 байт
  # С вероятностью 75% ожидание случайное от 0 до 111 миллисекунд ("probability-from-to")
  # Снова с вероятностью 50% отправка случайного padding от 0 до 3333 байт (если 0, то не Write())
  # Сервер и клиент могут устанавливать разные параметры padding, в порядке len, gap бесконечная цепочка, первый padding требует вероятность 100%, минимум 35 байт
  # -------------------------
  # decryption: "mlkem768x25519plus.native/xorpub/random.600s(300-600s)/0s.(padding len).(padding gap).(X25519 PrivateKey).(ML-KEM-768 Seed)..."
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
  reality-config:
    dest: test.com:443
    private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # можно сгенерировать командой mihomo generate reality-keypair
    short-id:
      - 0123456789abcdef
    server-names:
      - test.com
    #следующие два limit необязательны, могут ограничивать скорость fallback-соединений, не прошедших проверку, bytesPerSec по умолчанию 0 (не включено)
    #ограничение скорости fallback является характеристикой, не рекомендуется включать, если вы разработчик панели/скрипта, обязательно рандомизируйте эти параметры
    limit-fallback-upload:
      after-bytes: 0 # начать ограничение скорости после передачи указанного количества байт
      bytes-per-sec: 0 # базовая скорость (байт/сек)
      burst-bytes-per-sec: 0 # пиковая скорость (байт/сек), действует когда больше bytesPerSec
    limit-fallback-download:
      after-bytes: 0 # начать ограничение скорости после передачи указанного количества байт
      bytes-per-sec: 0 # базовая скорость (байт/сек)
      burst-bytes-per-sec: 0 # пиковая скорость (байт/сек), действует когда больше bytesPerSec
  ### Примечание: для vless listener необходимо заполнить по крайней мере один из параметров: "certificate и private-key" или "reality-config" или "decryption" ###
``` 