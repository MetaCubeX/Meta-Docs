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

### plugin

Плагин, поддерживает `obfs`/`v2ray-plugin`/`gost-plugin`/`shadow-tls`/`restls`/`kcptun`

### plugin-opts

Настройки плагина

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
        mode: websocket # на данный момент QUIC не поддерживается
          # tls: true # wss
          # Вы можете получить это с помощью команды: openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # Настройка fingerprint включает привязку SSL-сертификата (SSL pinning)
          # fingerprint: xxxx
          # skip-cert-verify: true
          # name-cert-verify: example.com # Изменяет только целевое имя DNSName для проверки сертификата, не меняя SNI.
          # host: bing.com
          # path: "/"
          # mux: true
          # headers:
          #   custom: value
          # v2ray-http-upgrade: false
    ```

=== "gost-plugin"
    ```{.yaml linenums="1"}
      plugin: gost-plugin
      plugin-opts:
        mode: websocket
          # tls: true # wss
          # Вы можете получить это с помощью команды: openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # Настройка fingerprint включает привязку SSL-сертификата (SSL pinning)
          # fingerprint: xxxx
          # skip-cert-verify: true # Изменяет только целевое имя DNSName для проверки сертификата, не меняя SNI.
          # name-cert-verify: example.com
          # host: bing.com
          # path: "/"
          # mux: true
          # headers:
          #   custom: value
    ```

=== "shadow-tls"
    ```{.yaml linenums="1"}
      plugin: shadow-tls
      client-fingerprint: chrome
      plugin-opts:
        host: "cloud.tencent.com"
        password: "shadow_tls_password"
        version: 2 # поддерживаются версии 1/2/3
    ```

=== "restls"
    ```{.yaml linenums="1"}
      plugin: restls
      client-fingerprint: chrome  # одно из значений: chrome, ios, firefox, safari
      plugin-opts:
        host: "[www.microsoft.com](https://www.microsoft.com)" # должен быть сервер с поддержкой TLS 1.3
        password: [YOUR_RESTLS_PASSWORD]
        version-hint: "tls13"
        # Управление трафиком после рукопожатия (post-handshake) через restls-script
        # Скрывает поведение прокси-сервера, такое как "tls в tls".
        # см. [https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md](https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md)
        restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
    ```

=== "kcptun"
    ```{.yaml linenums="1"}
      plugin: kcptun
      plugin-opts:
        key: it's a secrect # общий секрет (preshared secret) между клиентом и сервером
        crypt: aes # aes, aes-128, aes-128-gcm, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, none, null
        mode: fast # профили: fast3, fast2, fast, normal, manual
        conn: 1 # установить количество UDP-соединений с сервером
        autoexpire: 0 # установить время автоматического истечения срока действия (в секундах) для одного UDP-соединения, 0 — отключить
        scavengettl: 600 # установить время жизни истекшего соединения (в секундах)
        mtu: 1350 # установить максимальный размер единицы передачи (MTU) для UDP-пакетов
        ratelimit: 0 # установить максимальную исходящую скорость (в байтах в секунду) для одного KCP-соединения, 0 — отключить. Также известно как pacing пакетов
        sndwnd: 128 # установить размер окна отправки (количество пакетов)
        rcvwnd: 512 # установить размер окна приема (количество пакетов)
        datashard: 10 # установить кодирование стирания Рида-Соломона — datashard
        parityshard: 3 # установить кодирование стирания Рида-Соломона — parityshard
        dscp: 0 # установить DSCP (6 бит)
        nocomp: false # отключить сжатие
        acknodelay: false # немедленно отправлять ack (подтверждение) при получении пакета
        nodelay: 0
        interval: 50
        resend: 0
        sockbuf: 4194304 # буфер для каждого сокета в байтах
        smuxver: 1 # указать версию smux, доступны 1, 2
        smuxbuf: 4194304 # общий буфер демультиплексирования (de-mux) в байтах
        framesize: 8192 # максимальный размер кадра smux
        streambuf: 2097152 # буфер приема на один поток в байтах, для smux v2+
        keepalive: 10 # количество секунд между проверками активности (heartbeats)
    ```
