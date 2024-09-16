# 支持的类型

## UDP

```{.yaml linenums="1"}
- 223.5.5.5
- udp://223.5.5.5
```

## TCP

```{.yaml linenums="1"}
- tcp://8.8.8.8
```

## DNS over TLS

```{.yaml linenums="1"}
- tls://1.1.1.1
```

## DNS over HTTPS

```{.yaml linenums="1"}
- https://doh.pub/dns-query
```

## DNS over QUIC

```{.yaml linenums="1"}
- quic://dns.adguard.com:784
```

## system

```{.yaml linenums="1"}
- system://
- system
```

## dhcp

```{.yaml linenums="1"}
- dhcp://en0
```

仅限 cmfa，使用系统 dns

```{.yaml linenums="1"}
- dhcp://system
```

## rcode

```{.yaml linenums="1"}
- rcode://success            # No error
- rcode://format_error       # Format error
- rcode://server_failure     # Server failure
- rcode://name_error         # Non-existent domain
- rcode://not_implemented    # Not implemented
- rcode://refused            # Query refused
```
