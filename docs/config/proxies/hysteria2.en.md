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
  # bbr-profile: "" # Available: "standard", "conservative", "aggressive". Default: "standard"
  obfs: salamander # Defaults to empty. If filled, obfs will be enabled. Currently supports `salamander` and `gecko`.
  obfs-password: yourpassword
  # obfs-min-packet-size: 512
  # obfs-max-packet-size: 1200
  sni: server.com
  skip-cert-verify: false
  name-cert-verify: example.com
  fingerprint: xxxx # Configuring the fingerprint provides SSL pinning. Obtain it with: openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
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
  #   # The following supports entering TLS configuration for the server-url (sni, skip-cert-verify, name-cert-verify, fingerprint, certificate, private-key, alpn)
  #   # skip-cert-verify： false
  #   # name-cert-verify: example.com
  #   # ......
  # handshake-timeout: 30
  ###Special quic-go options. Do not modify them unless you know what you are doing.###
  # initial-stream-receive-window： 8388608
  # max-stream-receive-window： 8388608
  # initial-connection-receive-window： 20971520
  # max-connection-receive-window： 20971520
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

## handshake-timeout

Specified in seconds. When set, the handshake timeout is independent of the outer connection timeout. The default value is `0`, meaning the handshake uses only the outer connection timeout.
