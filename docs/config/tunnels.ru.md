# TUNNEL

Туннель для переадресации трафика, способный пересылать трафик TCP/UDP, а также может передаваться через прокси.

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,114.114.114.114:53,proxy
- network: [tcp, udp]
  address: 127.0.0.1:7777
  target: target.com
  proxy: proxy
```

## Однострочный вариант

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,8.8.8.8:53,proxy
```

## Многострочный вариант

```{.yaml linenums="1"}
tunnels:
- network: [tcp, udp]
  address: 127.0.0.1:6553
  target: 8.8.8.8:53
  proxy: proxy
```

В однострочном примере порядок соответствует многострочному варианту: `network`/`address`/`target`/`proxy`.

### network

Тип сети для прослушивания, может быть tcp/udp.

### address

Локальный адрес прослушивания.

### target

Целевой адрес для переадресации.

### proxy

Опционально, трафик отправляется через определенный `proxies`/`proxy-groups`.

В примере выше, доступ к `127.0.0.1:6553` перенаправляется через `proxy` из `proxies`/`proxy-groups` для доступа к `8.8.8.8:53`. 