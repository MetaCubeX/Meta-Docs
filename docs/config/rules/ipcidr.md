---
description: 有三种基于域名的规则,如果请求是域名,匹配IP规则则需要进行DNS查询(fake-ip)
---
## **IP-CIDR&IP-CIDR6**

IP 规则,请求的匹配指定的 IP 范围

```yaml
rules:
- IP-CIDR,127.0.0.0/8,DIRECT
- IP-CIDR6,2620:0:2d0:200::7/32,auto
```

```yaml

```

### **no-resolve**

当请求为域名匹配到 GEOIP 或 IP-CIDR 规则时,Clash 将请求 DNS 查询来检查域名的 IP 是否匹配此条规则,可以选择“`no-resolve`”选项以跳过域名去进行 dns 解析

如在更早的匹配中触发了解析,则依旧会匹配到添加了“`no-resolve`”选项的 IP 规则

如 `enhanced-mode`为 `redir-host`,则此选项无效

```yaml
rules:
- IP-CIDR,127.0.0.1/8,DIRECT,no-resolve
```

## **SRC-IP-CIDR**

来源 IP 规则,匹配请求的客户端 IP 地址

```yaml
rules:
- SRC-IP-CIDR,192.168.1.201/32,DIRECT
```
