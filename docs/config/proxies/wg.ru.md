# WireGuard

## Упрощенная запись

Если есть только один пир, можно использовать упрощенную запись.

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  server: 162.159.192.1
  port: 2480
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
  allowed-ips: ['0.0.0.0/0']
  # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
  # reserved: [209,98,59]  # Строковый формат также допустим, например "U4An"
  # persistent-keepalive: 0
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # Идентификатор исходящего прокси. Когда значение не пусто, используется указанный proxy/proxy-group для установки соединения
  # remote-dns-resolve: true # Принудительное удаленное разрешение DNS, по умолчанию false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Действует только когда remote-dns-resolve установлен в true
  # Если присутствует, активирует функции AmneziaWG
  # amnezia-wg-option:
  #   jc: 5
  #   jmin: 500
  #   jmax: 501
  #   s1: 30
  #   s2: 40
  #   h1: 123456
  #   h2: 67543
  #   h4: 32345
  #   h3: 123123
  #   # AmneziaWG v1.5
  #   i1: <b 0xf6ab3267fa><c><b 0xf6ab><t><r 10><wt 10>
  #   i2: <b 0xf6ab3267fa><r 100>
  #   i3: ""
  #   i4: ""
  #   i5: ""
  #   j1: <b 0xffffffff><c><b 0xf6ab><t><r 10>
  #   j2: <c><b 0xf6ab><t><wt 1000>
  #   j3: <t><b 0xf6ab><c><r 10>
  #   itime: 60
```

## Полная запись

Полная запись позволяет указать несколько пиров.

При использовании нескольких пиров `allowed-ips` каждого пира должны быть разными; в этом случае поля верхнего уровня `server, port, public-key, pre-shared-key, reserved` игнорируются, но `private-key` по-прежнему указывается на верхнем уровне.

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  peers:
    - server: 162.159.192.1
      port: 2480
      public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
      allowed-ips: ['0.0.0.0/0']
      # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
      # reserved: [209,98,59]  # Строковый формат также допустим, например "U4An"
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # Идентификатор исходящего прокси. Когда значение не пусто, используется указанный proxy/proxy-group для установки соединения
  # remote-dns-resolve: true # Принудительное удаленное разрешение DNS, по умолчанию false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Действует только когда remote-dns-resolve установлен в true
```

[Общие поля](./index.md)

### ip

IPv4-адрес, используемый локальной машиной в сети Wireguard

### ipv6

Необязательное поле, IPv6-адрес, используемый локальной машиной в сети Wireguard

### private-key

Закодированный в base64 приватный ключ клиента Wireguard

Можно использовать команду `wg genkey | tee privatekey | wg pubkey > publickey` для генерации пары рабочих ключей

### public-key

Закодированный в base64 публичный ключ сервера Wireguard

### allowed-ips

Необязательное поле, ограничивает, какие IP-диапазоны клиента будут перенаправляться через сервер. Обычно можно указать `['0.0.0.0/0']`

### pre-shared-key

Необязательное поле, предварительно общий ключ

### reserved

Необязательное поле, значение зарезервированного поля протокола Wireguard, необходимо для некоторых узлов WARP

### persistent-keepalive

Необязательное поле, периодически отправляющее пакеты данных для поддержания постоянного соединения.

### mtu

Необязательное поле, устанавливает значение MTU

### remote-dns-resolve

Необязательное поле, принудительное удаленное разрешение DNS, по умолчанию false

### dns

Необязательное поле, действует только когда `remote-dns-resolve` установлен в true, определяет DNS-сервер для удаленного разрешения

## Перевод из стандартного конфигурационного файла Wireguard

Предположим, у нас есть следующий стандартный конфигурационный файл Wireguard:

```ini
[Interface]
Address = <IP вашей машины в сети>
ListenPort = <Локальный порт прослушивания>
PrivateKey = <Ваш приватный ключ>
DNS = <Используемый DNS>
MTU = <Предустановленный MTU>

[Peer]
AllowedIPs = <Перенаправляемые IP-диапазоны>
Endpoint = <Удаленный адрес>:<Удаленный порт>
PublicKey = <Удаленный публичный ключ>
```

Соответствующая конфигурация узла для clash:

```{.yaml linenums="1"}
- name: "wg"
  type: wireguard
  ip: <IP вашей машины в сети, для IPv4>
  ipv6: <IP вашей машины в сети, для IPv6>  # Удалите, если нет v6-адреса
  private-key: <Ваш приватный ключ>
  peers:
    - server: <Удаленный адрес>
      port: <Удаленный порт>
      public-key: <Удаленный публичный ключ>
      allowed-ips: ['0.0.0.0/0']     # Разделение трафика обрабатывается clash
      # reserved: [209,98,59]        # Заполните при необходимости
  udp: true
  mtu: <Предустановленный MTU>     # Установите по необходимости, удалите если не нужно
  remote-dns-resolve: true          # Установите по необходимости, удалите если не нужно
  dns: <Используемый DNS>           # Установите по необходимости, удалите если не нужно
``` 
