# Экспериментальная конфигурация

```{.yaml linenums="1"}
experimental:
  quic-go-disable-gso: false
  quic-go-disable-ecn: false
  dialer-ip4p-convert: false
```

## quic-go-disable-gso

Отключить `GSO`

## quic-go-disable-ecn

Отключить `ECN`

## dialer-ip4p-convert

Включить преобразование адреса [IP4P](https://github.com/heiher/natmap/wiki/faq#域名访问是如何实现的) 