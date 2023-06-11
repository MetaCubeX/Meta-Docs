# 规则集

## RULE-SET

```yaml
rule-providers:
  google:
    type: http
    behavior: classical
    format: yaml
    # 由于安全问题，此路径将限制只允许在 HomeDir（有启动参数 -d 配置） 中，
    # 如果想存储到任意位置配置环境变量 `SKIP_SAFE_PATH_CHECK=1`
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
