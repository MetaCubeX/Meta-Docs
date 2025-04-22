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

Плагин, поддерживает `obfs`/`v2ray-plugin`/`gost-plugin`/`shadow-tls`/`restls`

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
        mode: websocket # QUIC пока не поддерживается
          # tls: true # wss
          # Можно получить с помощью openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # Настройка отпечатка реализует эффект SSL Pining
          # fingerprint: xxxx
          # skip-cert-verify: true
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
          # Можно получить с помощью openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
          # Настройка отпечатка реализует эффект SSL Pining
          # fingerprint: xxxx
          # skip-cert-verify: true
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
        version: 2 # поддерживает 1/2/3
    ```

=== "restls"
    ```{.yaml linenums="1"}
      plugin: restls
      client-fingerprint: chrome  # может быть одним из chrome, ios, firefox, safari
      plugin-opts:
        host: "www.microsoft.com" # должен быть сервер TLS 1.3
        password: [YOUR_RESTLS_PASSWORD]
        version-hint: "tls13"
        # Управляйте своим трафиком после рукопожатия через restls-script
        # Скрывайте поведение прокси, такое как "tls in tls".
        # см. https://github.com/3andne/restls/blob/main/Restls-Script:%20Hide%20Your%20Proxy%20Traffic%20Behavior.md
        restls-script: "300?100<1,400~100,350~100,600~100,300~200,300~100"
    ``` 