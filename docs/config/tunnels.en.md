Flow forwarding tunnel, capable of forwarding TCP/UDP traffic, and can also be forwarded through a proxy.

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,114.114.114.114:53,proxy
- network: [tcp, udp]
  address: 127.0.0.1:7777
  target: target.com
  proxy: proxy
```

## Single Line

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,8.8.8.8:53,proxy
```

## Multiple Lines

```{.yaml linenums="1"}
tunnels:
- network: [tcp, udp]
  address: 127.0.0.1:6553
  target: 8.8.8.8:53
  proxy: proxy
```

In the single line example, the order corresponds to the multiple lines: `network`/`address`/`target`/`proxy`.

### network

The type of network to listen to, can be tcp/udp.

### address

Local listening address.

### target

The forwarding target address.

### proxy

Optional, traffic sent through a specific `proxies`/`proxy-groups`.

In the above example, accessing `127.0.0.1:6553` is forwarded through the `proxy` in the `proxies`/`proxy-groups` to access `8.8.8.8:53`.
