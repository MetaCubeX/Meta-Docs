# Mieru

```{.yaml linenums="1"}
proxies:
  - name: mieru
    type: mieru
    server: server
    port: 2999
    port-range: 2090-2099
    transport: TCP
    username: user
    password: password
    multiplexing: MULTIPLEXING_LOW
    handshake-mode: HANDSHAKE_STANDARD
    traffic-pattern: ""
```

[Common fields](./index.md)

## port-range

Port range. Cannot be written together with `port`.

## transport

Protocol. Currently supports `TCP` and `UDP`.

## multiplexing

Multiplexing mode. Available values include `MULTIPLEXING_OFF`, `MULTIPLEXING_LOW`, `MULTIPLEXING_MIDDLE`, and `MULTIPLEXING_HIGH`. `MULTIPLEXING_OFF` disables multiplexing. Default: `MULTIPLEXING_LOW`.

## handshake-mode

Handshake mode. `HANDSHAKE_STANDARD` (default) waits for the handshake to complete; `HANDSHAKE_NO_WAIT` enables a 0-RTT handshake.

## traffic-pattern

A base64 string used to fine-tune network behavior. See the [official mieru documentation](https://github.com/enfein/mieru/blob/main/docs/traffic-pattern.md) for the format.
