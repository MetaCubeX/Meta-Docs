Clash.Meta использует входящий трафик и может работать как сервер.

## Входящие соединения из локальной сети

Входящие соединения для прослушивания трафика из локальной сети, подходит для передачи без шифрования:

```{.yaml linenums="1"}
listeners:
  - name: socks5-in-1
    type: socks
    port: 10808
    #listen: 0.0.0.0 # по умолчанию прослушивает 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy
    # udp: false # по умолчанию true

  - name: http-in-1
    type: http
    port: 10809
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)

  - name: mixed-in-1
    type: mixed #  смешанный прокси HTTP(S) и SOCKS
    port: 10810
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    # udp: false # по умолчанию true

  - name: reidr-in-1
    type: redir
    port: 10811
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)

  - name: tproxy-in-1
    type: tproxy
    port: 10812
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    # udp: false # по умолчанию true

  - name: tunnel-in-1
    type: tunnel
    port: 10816
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    network: [tcp, udp]
    target: target.com

  - name: tun-in-1
    type: tun
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    stack: system # gvisor / lwip
    dns-hijack:
      - 0.0.0.0:53 # DNS для перехвата
    # auto-detect-interface: false # автоматическое определение исходящего интерфейса
    # auto-route: false # настройка таблицы маршрутизации
    # mtu: 9000 # максимальная единица передачи
    inet4-address: # необходимо вручную задать диапазон IPv4-адресов
      - 198.19.0.1/30
    inet6-address: # необходимо вручную задать диапазон IPv6-адресов
      - "fdfe:dcba:9877::1/126"
    # strict-route: true # маршрутизация всех соединений через tun для предотвращения утечек, но устройство не будет доступно для других устройств
    #    inet4-route-address: # использование пользовательских маршрутов вместо маршрута по умолчанию при включенном auto-route
    #      - 0.0.0.0/1
    #      - 128.0.0.0/1
    #    inet6-route-address: # использование пользовательских маршрутов вместо маршрута по умолчанию при включенном auto-route
    #      - "::/1"
    #      - "8000::/1"
    # endpoint-independent-nat: false # включение независимого от точки NAT
    # include-uid: # правила UID поддерживаются только в Linux и требуют auto-route
    # - 0
    # include-uid-range: # ограничение диапазона пользователей для маршрутизации
    # - 1000-99999
    # exclude-uid: # исключение пользователей из маршрутизации
    #- 1000
    # exclude-uid-range: # исключение диапазона пользователей из маршрутизации
    # - 1000-99999

    # Правила для пользователей и приложений Android поддерживаются только на Android
    # и требуют auto-route

    # include-android-user: # ограничение пользователей Android для маршрутизации
    # - 0
    # - 10
    # include-package: # ограничение пакетов приложений Android для маршрутизации
    # - com.android.chrome
    # exclude-package: # исключение пакетов приложений Android из маршрутизации
    # - com.android.captiveportallogin
```

## Входящие соединения из интернета

Входящие соединения для шифрованной передачи трафика:

```{.yaml linenums="1"}
listeners:
  - name: shadowsocks-in-1
    type: shadowsocks
    port: 10813
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    password: vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=
    cipher: 2022-blake3-aes-256-gcm

  - name: vmess-in-1
    type: vmess
    port: 10814
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    users:
      - username: 1
        uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
        alterId: 1

  - name: tuic-in-1
    type: tuic
    port: 10815
    listen: 0.0.0.0
    # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
    # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
    # token:    # для tuicV4 (нельзя одновременно указывать token и users)
    #   - TOKEN
    # users:    # для tuicV5 (нельзя одновременно указывать token и users)
    #   00000000-0000-0000-0000-000000000000: PASSWORD-0
    #   00000000-0000-0000-0000-000000000001: PASSWORD-1
    #  certificate: ./server.crt
    #  private-key: ./server.key
    #  congestion-controller: bbr
    #  max-idle-time: 15000
    #  authentication-timeout: 1000
    #  alpn:
    #    - h3
    #  max-udp-relay-packet-size: 1500


```

!!! note
    Если proxy не пусто, то входящий трафик передается указанному [proxy](../proxies/index.md)

    Если определенное [sub-rule](../sub-rule.md) не существует, то напрямую используются rules

## Конфигурация входов

Конфигурация входов эквивалентна Listener, входящий трафик будет обрабатываться так же, как и через входы socks, mixed и т.д., в соответствии с указанным режимом

```{.yaml linenums="1"}
# конфигурация shadowsocks, vmess (входящий трафик будет обрабатываться так же, как и через входы socks, mixed и т.д., в соответствии с указанным режимом)
ss-config: ss://2022-blake3-aes-256-gcm:vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=@:23456
vmess-config: vmess://1:9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68@:12345

# сервер tuic (входящий трафик будет обрабатываться так же, как и через входы socks, mixed и т.д., в соответствии с указанным режимом)
tuic-server:
 enable: true
 listen: 127.0.0.1:10443
 token:    # для tuicV4 (нельзя одновременно указывать token и users)
   - TOKEN
 users:    # для tuicV5 (нельзя одновременно указывать token и users)
   00000000-0000-0000-0000-000000000000: PASSWORD-0
   00000000-0000-0000-0000-000000000001: PASSWORD-1
 certificate: ./server.crt
 private-key: ./server.key
 congestion-controller: bbr
 max-idle-time: 15000
 authentication-timeout: 1000
 alpn:
   - h3
 max-udp-relay-packet-size: 1500
``` 