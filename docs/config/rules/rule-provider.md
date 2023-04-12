# 规则集

## `RULE-SET`

```yaml
rule-providers:
  google:
    type: http
    behavior: classical
    path: ./rule1.yaml
    #【Meta专属】URL可根据rule设定匹配对应的策略，方便更新provider
    url: "https://raw.githubusercontent.com/../Google.yaml"
    interval: 600
```