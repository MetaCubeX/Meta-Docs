# Snell

```{.yaml linenums="1"}
proxies
- name: "snell"
  type: snell
  server: server
  port: 44046
  psk: yourpsk
  version: 3
  obfs-opts:
    mode: http
    host: bing.com
```

[Common fields](./index.md)

## psk

Required, Snell pre-shared key.

## version

Snell version. Only v1-3 are supported. Default: v1. Only v3 supports UDP.

## obfs-opts

Snell obfuscation settings.

### obfs-opts.mode

Snell obfuscation mode. Supports `http` and `tls`.

### obfs-opts.host

Snell obfuscation hostname.
