# 子规则

## SUB-RULE

匹配到规则时,将请求送往另一规则流程,括号内可以使用任意规则

```yaml
rules:
- SUB-RULE,(NETWORK,UDP),rule1
```

[流程示例](../sub-rules.md)
