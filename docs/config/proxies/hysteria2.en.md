# Hysteria2

[Configuration Reference](https://hysteria.network/docs/advanced/Full-Client-Config/)

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
  obfs: salamander
  obfs-password: yourpassword
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx
  alpn:
    - h3
```

[Common Fields](./index.md)

[TLS Fields](./tls.md)

## ports

Configuring this enables port jumping, ignoring `port`. Refer to [Port Range](../../handbook/syntax.md#port-ranges) for format.

## hop-interval

Port hop interval, in seconds, default is 30.

Entering "15-30" will randomly select one value as the switching interval each time. Only one range is supported (commas are not allowed).

## password

Authentication password.

## up/down

Brutal rate control; if no unit is specified, the default is Mbps.

## obfs

QUIC traffic obfuscator type. Can be set to `salamander` or `gecko`. If left blank, obfuscation is disabled.

## obfs-password

QUIC traffic obfuscator password.

## obfs-min-packet-size

Minimum wire packet size (in bytes). Restricted to `gecko` only.

## obfs-max-packet-size

Maximum wire packet size (in bytes). Restricted to `gecko` only.

## realm-opts
