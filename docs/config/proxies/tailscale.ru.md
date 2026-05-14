# Tailscale

```{.yaml linenums="1"}
proxies:
  - name: "tailscale"
    type: tailscale
    hostname: mihomo
    auth-key: tskey-auth-xxxx
    control-url: https://controlplane.tailscale.com
    state-dir: ./tailscale
    ephemeral: false
    udp: true
    accept-routes: true
    exit-node: 100.64.0.1
    exit-node-allow-lan-access: true
    dialer-proxy: "ss1"
    interface-name: "WLAN"
    routing-mark: 6666
    ip-version: ipv4-prefer
```

## name

Обязательно, имя прокси. Не должно повторяться.

## type

Обязательно, фиксированное значение `tailscale`.

## hostname

Опционально, имя устройства Tailscale. Если не указано, обрабатывается `tsnet`.

## auth-key

Опционально, ключ входа Tailscale или Headscale. Если не указано, при первом запуске в логах будет выведен URL для интерактивного входа.

## control-url

Опционально, адрес пользовательского Tailscale control server или Headscale.

## state-dir

Опционально, каталог состояния `tsnet`; значение по умолчанию — `tailscale`.

## ephemeral

Опционально, входить ли как ephemeral node. Значение по умолчанию — `false`.

## udp

Опционально, включить ли UDP. Значение по умолчанию — `false`.

## accept-routes

Опционально, принимать ли subnet routes, опубликованные в Tailnet.

## exit-node

Опционально, использовать указанный exit node. Можно указать IP узла, также поддерживается `auto:any`.

Если цель не входит в маршруты Tailscale, соединение завершится ошибкой и не будет переключено на прямое подключение. Для доступа к публичному интернету нужен доступный `exit-node` или принятые subnet routes, покрывающие целевую сеть.

## exit-node-allow-lan-access

Опционально, разрешить ли доступ к локальной LAN при использовании exit node.

## dialer-proxy

Опционально, идентификатор исходящего прокси. Если значение не пустое, указанный proxy будет использоваться для соединений Tailscale control plane, DERP/STUN и других служебных соединений.

## interface-name

Опционально, указать исходящий сетевой интерфейс.

## routing-mark

Опционально, настроить fwmark в Linux.

## ip-version

Опционально, указать версию IP для исходящих соединений.

Возможные значения: `dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer`.
