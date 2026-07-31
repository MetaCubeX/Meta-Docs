# Shadowsocks

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  type: ss
  server: server
  port: 443
  cipher: aes-128-gcm
  password: "password"
  udp: true
  udp-over-tcp: false
  udp-over-tcp-version: 2
  ip-version: ipv4
  plugin: obfs
  plugin-opts:
    mode: tls
  smux:
    enabled: false
```

[Общие поля](./index.md)

## Cipher

=== "AES"
    |          Метод               |                            |                            |
    |----------------------------|----------------------------|----------------------------|
    | aes-128-ctr                | aes-192-ctr                | aes-256-ctr                |
    | aes-128-cfb                | aes-192-cfb                | aes-256-cfb                |
    | aes-128-gcm                | aes-192-gcm                | aes-256-gcm                |
    | aes-128-ccm                | aes-192-ccm                | aes-256-ccm                |
    | aes-128-gcm-siv            |                            | aes-256-gcm-siv            |

=== "CHACHA"
    | Метод                           |                                |
    |--------------------------------|--------------------------------|
    | chacha20-ietf                  |                                |
    | chacha20                       | xchacha20                      |
    | chacha20-ietf-poly1305         | xchacha20-ietf-poly1305        |
    | chacha8-ietf-poly1305          | xchacha8-ietf-poly1305         |

=== "2022 Blake3"
    | Метод                                |
    |-------------------------------------|
    | 2022-blake3-aes-128-gcm             |
    | 2022-blake3-aes-256-gcm             |
    | 2022-blake3-chacha20-poly1305       |

=== "LEA"
    | Метод               |
    |--------------------|
    | lea-128-gcm        |
    | lea-192-gcm        |
    | lea-256-gcm        |

=== "Другие"
    | Метод               |
    |--------------------|
    | rabbit128-poly1305|
    | aegis-128l         |
    | aegis-256          |
    | aez-384            |
    | deoxys-ii-256-128  |
    | rc4-md5            |
    | none               |

## password

Пароль Shadowsocks

## udp-over-tcp

Включить UDP over TCP, по умолчанию false

## udp-over-tcp-version

Версия протокола UDP over TCP, по умолчанию 1. Возможные значения: 1/2.

## Плагины

`plugin` задаёт тип плагина: `obfs`, `v2ray-plugin`, `gost-plugin`, `shadow-tls`, `restls`, `kcptun` или `jls`. `plugin-opts` содержит настройки выбранного плагина.

=== "obfs"

    ```{.yaml linenums="1"}
    plugin: obfs
    plugin-opts:
      mode: tls
      host: bing.com
    ```

=== "v2ray-plugin"
    ```{.yaml linenums="1"}
    plugin: v2ray-plugin
    plugin-opts:
      mode: websocket
      tls: true
      fingerprint: xxxx
      skip-cert-verify: true
      name-cert-verify: example.com
      host: bing.com
      path: "/"
      mux: true
      headers:
        custom: value
      v2ray-http-upgrade: false
    ```

    `mode` фиксирован как `websocket`; QUIC пока не поддерживается.

    - `tls`: включает WSS.
    - `fingerprint`: SHA-256 отпечаток сертификата. Его можно получить командой `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem`; при настройке включается SSL pinning.
    - `skip-cert-verify`: отключает проверку сертификата.
    - `name-cert-verify`: домен для проверки сертификата.
    - `host`: WebSocket `Host`.
    - `path`: путь WebSocket.
    - `mux`: включает мультиплексирование.
    - `headers`: пользовательские заголовки запроса WebSocket.
    - `v2ray-http-upgrade`: включает V2Ray HTTP Upgrade.

=== "gost-plugin"
    ```{.yaml linenums="1"}
    plugin: gost-plugin
    plugin-opts:
      mode: websocket
      tls: true
      fingerprint: xxxx
      skip-cert-verify: true
      name-cert-verify: example.com
      host: bing.com
      path: "/"
      mux: true
      headers:
        custom: value
    ```

    - `tls`: включает WSS.
    - `fingerprint`: SHA-256 отпечаток сертификата. Его можно получить командой `openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem`; при настройке включается SSL pinning.
    - `skip-cert-verify`: отключает проверку сертификата.
    - `name-cert-verify`: домен для проверки сертификата.
    - `host`: WebSocket `Host`.
    - `path`: путь WebSocket.
    - `mux`: включает мультиплексирование.
    - `headers`: пользовательские заголовки запроса WebSocket.

=== "shadow-tls"
    ```{.yaml linenums="1"}
    plugin: shadow-tls
    client-fingerprint: chrome
    plugin-opts:
      host: "cloud.tencent.com"
      password: "shadow_tls_password"
      version: 2
    ```

    `version` поддерживает `1`, `2` и `3`.

=== "restls"
    ```{.yaml linenums="1"}
    plugin: restls
    client-fingerprint: chrome
    plugin-opts:
      host: "www.microsoft.com"
      password: [YOUR_RESTLS_PASSWORD]
      version-hint: "tls13"
      restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
    ```

    - `client-fingerprint`: `chrome`, `ios`, `firefox` или `safari`.
    - `plugin-opts.host`: должен быть сервером TLS 1.3.
    - `plugin-opts.restls-script`: управляет трафиком Restls после рукопожатия и скрывает признаки прокси, например «TLS в TLS»; см. [документацию скрипта](https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md).

=== "kcptun"
    ```{.yaml linenums="1"}
    plugin: kcptun
    plugin-opts:
      key: it's a secrect
      crypt: aes
      mode: fast
      conn: 1
      autoexpire: 0
      scavengettl: 600
      mtu: 1350
      ratelimit: 0
      sndwnd: 128
      rcvwnd: 512
      datashard: 10
      parityshard: 3
      dscp: 0
      nocomp: false
      acknodelay: false
      nodelay: 0
      interval: 50
      resend: 0
      sockbuf: 4194304
      smuxver: 1
      smuxbuf: 4194304
      framesize: 8192
      streambuf: 2097152
      keepalive: 10
    ```

    | Поле | Описание |
    | --- | --- |
    | `key` | Предварительно общий секрет клиента и сервера |
    | `crypt` | Шифр: `aes`, `aes-128`, `aes-128-gcm`, `aes-192`, `salsa20`, `blowfish`, `twofish`, `cast5`, `3des`, `tea`, `xtea`, `xor`, `none`, `null` |
    | `mode` | Предустановленный профиль: `fast3`, `fast2`, `fast`, `normal`, `manual` |
    | `conn` | Количество UDP-соединений с сервером |
    | `autoexpire` | Автоматическое истечение одного UDP-соединения в секундах; `0` отключает его |
    | `scavengettl` | Время жизни истекшего соединения в секундах |
    | `mtu` | Максимальная единица передачи для UDP-пакетов |
    | `ratelimit` | Максимальная исходящая скорость KCP-соединения в байтах/с; `0` отключает её. Также называется packet pacing |
    | `sndwnd` / `rcvwnd` | Размер окна отправки/приёма в пакетах |
    | `datashard` / `parityshard` | Число data/parity частей для коррекции ошибок Reed-Solomon |
    | `dscp` | Значение DSCP (6 бит) |
    | `nocomp` | Отключает сжатие |
    | `acknodelay` | Немедленно отправляет ACK при получении пакета |
    | `nodelay` / `interval` / `resend` | Режим KCP no-delay, интервал обновления и параметры быстрой повторной передачи |
    | `sockbuf` | Буфер одного socket в байтах |
    | `smuxver` | Версия smux: `1` или `2` |
    | `smuxbuf` | Общий буфер демультиплексирования в байтах |
    | `framesize` | Максимальный размер кадра smux в байтах |
    | `streambuf` | Буфер приёма одного stream в байтах, для smux v2+ |
    | `keepalive` | Интервал между heartbeat-пакетами в секундах |

=== "jls"
    ```{.yaml linenums="1"}
    plugin: jls
    client-fingerprint: chrome
    plugin-opts:
      host: "www.example.com"
      username: "jls-user"
      password: "jls-password"
      alpn: [h2, http/1.1]
    ```

    `plugin-opts.alpn` опционально. Здесь указывается список прикладных протоколов, например `[h2, http/1.1]`.
