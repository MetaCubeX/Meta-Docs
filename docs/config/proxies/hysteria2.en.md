# Hysteria2

[Configuration Reference](https://hysteria.network/en/docs/advanced-usage/#client-side)

```{.yaml linenums="1"}
proxies:
- name: "hysteria2"
  type: hysteria2
  server: server.com
  port: 443
  ports: 443-8443
  hop-interval: 30
  password: yourpassword
  up: "30 Mbps"
  down: "200 Mbps"
  obfs: salamander # Default is empty; if filled, obfs is enabled. Currently, only salamander is supported.
  obfs-password: yourpassword

  sni: server.com
  skip-cert-verify: false
  fingerprint: xxxx
  alpn:
    - h3
```

[Common Fields](./index.md)

[TLS Fields](./tls.md)

## ports

Configuring this enables port jumping, ignoring `port`. Refer to [Port Range](../../handbook/syntax.md#port-ranges) for format.

## password

Authentication password.

## hop-interval

Port hop interval, in seconds, default is 30.

## up/down

Brutal rate control; if no unit is specified, the default is Mbps.

## obfs

QUIC traffic obfuscator type, can only be set to `salamander`. If left empty, it is disabled.

## obfs-password

QUIC traffic obfuscator password.
