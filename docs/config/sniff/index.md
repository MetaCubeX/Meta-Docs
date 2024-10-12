# 域名嗅探

```{.yaml linenums="1"}
sniffer:
  enable: false
  force-dns-mapping: true
  parse-pure-ip: true
  override-destination: false
  sniff:
    HTTP:
      ports: [80, 8080-8880]
      override-destination: true
    TLS:
      ports: [443, 8443]
    QUIC:
      ports: [443, 8443]
  force-domain:
    - +.v2ex.com
  skip-domain:
    - Mijia Cloud
  skip-src-address:
    - 192.168.0.3/32
  skip-dst-address:
    - 192.168.0.3/32
```

## enable

是否启用 sniffer

## force-dns-mapping

对 `redir-host` 类型识别的流量进行强制嗅探

## parse-pure-ip

对所有未获取到域名的流量进行强制嗅探

## override-destination

是否使用嗅探结果作为实际访问，默认为 true

## sniff

需要嗅探的协议设置，仅支持 `HTTP`/`TLS`/`QUIC`

- `ports`: [端口范围](../../handbook/syntax.md#_14)
- `override-destination`: 覆盖全局`override-destination`设置

## force-domain

强制进行嗅探的域名列表，使用[域名通配](../../handbook/syntax.md#_8)

## skip-domain

跳过嗅探的域名列表，使用[域名通配](../../handbook/syntax.md#_8)

## skip-src-address

跳过嗅探的来源 IP 段列表

## skip-dst-address

跳过嗅探的目标 IP 段列表
