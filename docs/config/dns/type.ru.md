# Поддерживаемые типы

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

Только для cmfa, использует системный dns

```{.yaml linenums="1"}
- dhcp://system
```

## rcode

```{.yaml linenums="1"}
- rcode://success            # Нет ошибки
- rcode://format_error       # Ошибка формата
- rcode://server_failure     # Ошибка сервера
- rcode://name_error         # Несуществующий домен
- rcode://not_implemented    # Не реализовано
- rcode://refused            # Запрос отклонен
``` 