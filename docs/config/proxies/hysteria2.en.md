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
  # bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  obfs: salamander # Default is empty; if filled, obfs is enabled. Currently, only salamander is supported.
  obfs-password: yourpassword

  sni: server.com
  skip-cert-verify: false
  fingerprint: xxxx
  alpn:
    - h3
  # realm-opts:
  #   enable: true # Must be turned on manually
  #   server-url: https://realm.hy2.io
  #   token: public
  #   realm-id: my-cabin-1f3a8c2e9b
  #   stun-servers:
  #     - stun.nextcloud.com:3478
  #     - stun.sip.us:3478
  #     - global.stun.twilio.com:3478
  #   # The following supports entering TLS configuration for the server-url (sni, skip-cert-verify, fingerprint, certificate, private-key, alpn)
  #   # skip-cert-verify： false
  #   # ......
```

[Common Fields](./index.md)

[TLS Fields](./tls.md)

## ports

Configuring this enables port jumping, ignoring `port`. Refer to [Port Range](../../handbook/syntax.md#port-ranges) for format.

## password

Authentication password.

## hop-interval

Port hop interval, in seconds, default is 30.

Entering "15-30" will randomly select one of the values ​​as the switching interval each time. Only a range is supported (commas are not allowed).

## up/down

Brutal rate control; if no unit is specified, the default is Mbps.

## obfs

QUIC traffic obfuscator type, can only be set to `salamander`. If left empty, it is disabled.

## obfs-password

QUIC traffic obfuscator password.

## realm-opts
