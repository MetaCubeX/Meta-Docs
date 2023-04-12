# 域名嗅探

Clash使用Mapping机制解决透明代理情况下,无法通过Redir端口传递域名的问题；但此机制会导致如果不使用Clash内置的DNS解析服务,就无法准确还原域名,进行域名分流的问题。

Meta内置了Sniffer域名嗅器,通过读取握手包内的域名字段,将IP还原成域名,有效解决Mapping机制的短板。

```yaml
sniffer:                           #【Meta专属】sniffer域名嗅探器
  enable: false                    # 嗅探器开关       
                                   # 开启后对 redir-host 类型识别的流量进行强制嗅探
                                   # 包含 Tun、Redir 和 TProxy 或 DNS 为 redir-host
  # force-dns-mapping: false
  # parse-pure-ip: false           # 对所有未获取到域名的流量进行强制嗅探
  
  override-destination: false      # 是否使用嗅探结果作为实际访问,默认 true
                                   # 全局配置,优先级低于 sniffer.sniff 实际配置
  sniff: # TLS 默认如果不配置 ports 默认嗅探 443
    TLS:
      ports: [443, 8443]
    HTTP: # 需要嗅探的端口, 默认嗅探 80
      ports: [80, 8080-8880]
      override-destination: true # 可覆盖 sniffer.override-destination
  force-domain:
    - +.v2ex.com
  # 白名单,跳过嗅探结果
  skip-domain:
    - Mijia Cloud
```
