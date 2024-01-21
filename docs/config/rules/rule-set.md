```{.yaml linenums="1"}
rules:
  - RULE-SET,google,PROXY
```

在 [nameserver-policy](../dns/index.md) 中使用

```{.yaml linenums="1"}
dns:
 nameserver-policy:
  "rule-set:global,dns": 8.8.8.8
```