# ShadowSocks

```{.yaml linenums="1"}
listeners:
- name: ss-in
  type: shadowsocks
  port: 10001
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
  cipher: 2022-blake3-aes-256-gcm
  password: vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=
  udp: true
  # simple-obfs:
  #   enable: false # Установите true для включения
  #   mode: http # Возможные значения: http, tls
  # shadow-tls:
  #   enable: false
  #   version: 3 # v1/v2/v3
  #   password: password # v2
  #   users: # v3
  #     - name: 1
  #       password: password
  #   handshake:
  #     dest: test.com:443
  # res-tls:
  #   enable: false
  #   dest: test.com:443
  #   password: password
  #   restls-script: ""
  #   min-record-len: 0
  #   proxy: ""
  # jls-config: # Только оборачивает TCP; при ошибке JLS-аутентификации или обычном TLS соединение прозрачно возвращается на dest
  #   enable: false
  #   users:
  #     - username: jls-user
  #       password: jls-password
  #   dest: www.example.com:443
  #   # sni: www.example.com # Если пусто, выводится из dest
  #   # alpn: [h2, http/1.1]
  #   # proxy: ""
  #   # rate-limit: 0 # Ограничение скорости пересылки, bit/s; 0 означает без ограничения
  # kcp-tun:
  #   enable: false
  #   key: it's a secrect # предварительно согласованный секрет клиента и сервера
  #   crypt: aes # aes, aes-128, aes-128-gcm, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, none, null
  #   mode: fast # профили: fast3, fast2, fast, normal, manual
  #   conn: 1 # количество UDP-соединений с сервером
  #   autoexpire: 0 # время автоматического истечения одного UDP-соединения, в секундах; 0 отключает
  #   scavengettl: 600 # время жизни истекшего соединения, в секундах
  #   ratelimit: 0 # максимальная исходящая скорость одного KCP-соединения, в байтах/с; 0 отключает. Также называется packet pacing
  #   mtu: 1350 # максимальная единица передачи для UDP-пакетов
  #   sndwnd: 128 # размер окна отправки, в пакетах
  #   rcvwnd: 512 # размер окна приема, в пакетах
  #   datashard: 10 # число блоков данных для кодирования Рида-Соломона
  #   parityshard: 3 # число блоков четности для кодирования Рида-Соломона
  #   dscp: 0 # значение DSCP, 6 бит
  #   nocomp: false # отключить сжатие
  #   acknodelay: false # немедленно отправлять ACK при получении пакета
  #   nodelay: 0
  #   interval: 50
  #   resend: 0
  #   sockbuf: 4194304 # буфер одного сокета, в байтах
  #   smuxver: 1 # версия smux, возможные значения: 1, 2
  #   smuxbuf: 4194304 # общий буфер демультиплексирования, в байтах
  #   framesize: 8192 # максимальный размер кадра smux
  #   streambuf: 2097152 # буфер приема одного потока, в байтах; smux v2+
  #   keepalive: 10 # интервал между heartbeat-пакетами, в секундах
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # по умолчанию в Mbps
  #     down: 1000
```

## [Общие поля](./index.md)

## Настройка протокола

### cipher

Метод шифрования

| Метод                          | Длина пароля |
| ----------------------------- | -------- |
| 2022-blake3-aes-128-gcm       | 16       |
| 2022-blake3-aes-256-gcm       | 32       |
| 2022-blake3-chacha20-poly1305 | 32       |
| none                          | /        |
| aes-128-gcm                   | /        |
| aes-192-gcm                   | /        |
| aes-256-gcm                   | /        |
| chacha20-ietf-poly1305        | /        |
| xchacha20-ietf-poly1305       | /        |

### password

| Метод | Формат пароля                        |
| ---- | ------------------------------------ |
| none | /                                    |
| 2022 | `openssl rand --base64 <длина пароля>` |
| другие | любая строка                           |

### udp

Прослушивать ли UDP
