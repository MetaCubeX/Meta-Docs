# ZeroTier

```{.yaml linenums="1"}
proxies:
- name: "zerotier"
  type: zerotier
  network: "0123456789abcdef"
  state-dir: ./zerotier-node
  planet: ./planet
  mtu: 1400
  physical-mtu: 1432
  primary-port: 0
  secondary-port: 0
  tcp-fallback-mode: auto
  tcp-fallback-relay: 204.80.128.1:443
  remote-trace-target: "0123456789"
  remote-trace-level: 0
  low-bandwidth: false
  encrypted-hello: false
  orbit:
    - world: "0123456789abcdef"
      seed: "0123456789"
  udp: true
  remote-dns-resolve: true
  dns: [10.147.17.1]
  dialer-proxy: "ss1"
  interface-name: "WLAN"
  routing-mark: 6666
  ip-version: ipv4-prefer
```

[Общие поля](./index.md)

## network

Обязательно, 16-значный шестнадцатеричный ZeroTier Network ID.

## state-dir

Опционально, каталог для постоянного хранения идентификатора узла. По умолчанию для каждого Network ID и имени исходящего прокси создаётся изолированный каталог в `zerotier`.

## planet

Опционально, путь к пользовательскому файлу planet. При указании заменяет встроенный Earth planet.

## mtu

Опционально, локальное ограничение MTU. Диапазон значений: от `1280` до `10000`; фактический MTU не превышает значение, назначенное контроллером.

## physical-mtu

Опционально, MTU UDP-пакета ZeroTier. Диапазон значений: от `510` до `10324`; значение по умолчанию: `1432`.

## primary-port

Опционально, основной UDP-порт. Диапазон значений: от `0` до `65535`; `0` автоматически выбирает доступный порт.

## secondary-port

Опционально, второй UDP-порт. Диапазон значений: от `-1` до `65535`; `0` автоматически выбирает доступный порт, `-1` отключает второй порт.

## tcp-fallback-mode

Опционально, режим TCP fallback. Значение по умолчанию: `auto`.

Доступные значения:

- `auto`: использовать TCP relay после сбоя прямого UDP.
- `force`: использовать только TCP relay.
- `disable`: отключить TCP relay.

## tcp-fallback-relay

Опционально, адрес TCP fallback relay в формате `host:port`. По умолчанию используется публичный relay ZeroTier.

## remote-trace-target

Опционально, 10-значный шестнадцатеричный ZeroTier node ID, который получает глобальные диагностические traces.

## remote-trace-level

Опционально, уровень удалённой диагностической трассировки. Диапазон значений: от `0` до `30`.

Доступные значения: `0` (normal)/`10` (verbose)/`15` (rules)/`20` (debug)/`30` (insane).

## low-bandwidth

Опционально, уменьшать ли фоновый трафик и частоту обновления конфигурации сети. Значение по умолчанию: `false`.

## encrypted-hello

Опционально, включать ли расширенное шифрование protocol-13 для исходящих HELLO-пакетов. Значение по умолчанию: `false`.

## orbit

Опционально, список федеративных корневых миров (moons).

### orbit.world

Обязательно, 16-значный шестнадцатеричный moon world ID.

### orbit.seed

Обязательно, 10-значный шестнадцатеричный node ID корня moon.

## remote-dns-resolve

Опционально, разрешать ли имена назначений через ZeroTier с DNS-серверами, переданными контроллером. Значение по умолчанию: `false`.

## dns

Опционально, действует только при `remote-dns-resolve: true`. При указании заменяет DNS-серверы, переданные контроллером.
