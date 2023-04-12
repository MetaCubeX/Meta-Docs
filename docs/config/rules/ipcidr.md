---
description: 有三种基于域名的规则,如果请求是域名,匹配IP规则则需要进行DNS查询(fake-ip)
---

# IP规则

## **IP-CIDR&IP-CIDR6**

IP规则,请求的匹配指定的IP范围

```
IP-CIDR,127.0.0.0/8,DIRECT
IP-CIDR6,2620:0:2d0:200::7/32,auto
```

### **no-resolve**

当请求为域名匹配到GEOIP或IP-CIDR规则时,clash将请求DNS查询来检查域名的IP是否匹配此条规则,可以选择“`no-resolve`”选项以跳过域名去进行dns解析

如在更早的匹配中触发了解析,则依旧会匹配到添加了“`no-resolve`”选项的IP规则

如`enhanced-mode`为`redir-host`,则此选项无效

```
IP-CIDR,127.0.0.1/8,DIRECT,no-resolve
```

## **GEOIP**

国家IP代码规则,匹配集合内相应的IP范围

```
GEOIP,cn,DIRECT
GEOIP,lan,DIRECT
```

### **no-resolve**

关于 [no-resolve](ipcidr.md#no-resolve)

```
GEOIP,lan,DIRECT,no-resolve
```

## **SRC-IP-CIDR**

来源IP规则,匹配请求的客户端IP地址

```
SRC-IP-CIDR,192.168.1.201/32,DIRECT
```
