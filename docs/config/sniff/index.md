# 域名嗅探

Clash使用Mapping机制解决透明代理情况下,无法通过Redir端口传递域名的问题；但此机制会导致如果不使用Clash内置的DNS解析服务,就无法准确还原域名,进行域名分流的问题。

Meta内置了Sniffer域名嗅器,通过读取握手包内的域名字段,将IP还原成域名,有效解决Mapping机制的短板。

## 示例

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
```

## 字段解释

### enable

是否启用sniffer

### force-dns-mapping

对 redir-host 类型识别的流量进行强制嗅探

### parse-pure-ip

对所有未获取到域名的流量进行强制嗅探

### override-destination

是否使用嗅探结果作为实际访问，默认为 true

### sniff

一个数组，里面可以包含多个协议对象。每种协议对象包含：

- `ports`字段，表示端口范围。示例：`ports: [80, 8080-8880]`
- `override-destination`字段（可选），用于覆盖全局`override-destination`设置

### force-domain

需要强制嗅探的域名（默认情况下只对IP进行嗅探）

### skip-domain

需要跳过嗅探的域名。主要解决部分站点sni字段非域名，导致嗅探结果异常的问题，如米家设备`Mijia Cloud`
