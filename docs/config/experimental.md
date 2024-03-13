# 实验性配置

```{.yaml linenums="1"}
experimental:
  quic-go-disable-gso: false
  quic-go-disable-ecn: false
  dialer-ip4p-convert: false
```

## quic-go-disable-gso

禁用`GSO`

## quic-go-disable-ecn

禁用`ECN`

## dialer-ip4p-convert

启用[IP4P](https://github.com/heiher/natmap/wiki/faq#域名访问是如何实现的)地址转换
