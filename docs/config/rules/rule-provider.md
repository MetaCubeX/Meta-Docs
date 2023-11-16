# 规则集

## RULE-SET

```yaml
rule-providers:
  google:
    type: http
    behavior: classical       # domain / ipcidr / classical
    format: yaml
    # 由于安全问题，此路径将限制只允许在 HomeDir（有启动参数 -d 配置） 中，
    # 如果想存储到任意位置配置环境变量 `SKIP_SAFE_PATH_CHECK=1`
    # path可为空(仅限clash.meta 1.15.0以上版本)
    path: ./rule1.yaml 
    #【Meta专属】URL可根据rule设定匹配对应的策略，方便更新provider
    url: "https://raw.githubusercontent.com/../Google.yaml"
    interval: 600
```

在规则中引用

```yaml
rules:
  - RULE-SET,google,PROXY
```

在 [nameserver-policy](../dns/index.md) 中引用

```yaml
dns:
 nameserver-policy:
  "rule-set:global,dns": 8.8.8.8
```

## rule-set文件格式

`behavior`参数有三种可选项：`domain` / `ipcidr` / `classical`，对应不同格式的rule-set文件格式。请按实际格式填写

### domain

```yaml
payload:
  - '.blogger.com'
  - '*.*.microsoft.com'
  - 'books.itunes.apple.com'
  ...
```

### ipcidr

```yaml
payload:
  - '192.168.1.0/24'
  - '10.0.0.0.1/32'
  ...
```

### classical

```yaml
payload:
  - DOMAIN-SUFFIX,google.com
  - DOMAIN-KEYWORD,google
  - DOMAIN,ad.com
  - SRC-IP-CIDR,192.168.1.201/32
  - IP-CIDR,127.0.0.0/8
  - GEOIP,CN
  - DST-PORT,80
  - SRC-PORT,7777
  ...
```