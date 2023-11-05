# 域名嗅探

Clash使用Mapping机制解决透明代理情况下,无法通过Redir端口传递域名的问题；但此机制会导致如果不使用Clash内置的DNS解析服务,就无法准确还原域名,进行域名分流的问题。

Meta内置了Sniffer域名嗅器,通过读取握手包内的域名字段,将IP还原成域名,有效解决Mapping机制的短板。

## 示例

```yaml
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
```

### enbale
