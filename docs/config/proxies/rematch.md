# Rematch

```{.yaml linenums="1"}
proxies:
- name: "rematch"
  type: rematch
  target-rematch-name: "rematch1"
  target-sub-rule: "sub-rule1"
```

[通用字段](./index.md)

## target-rematch-name

覆盖原始 metadata 中的 rematch-name，可使用 [REMATCH-NAME](../rules/index.md#rematch-name) 规则匹配

## target-sub-rule

如果填写，后续会直接使用指定的 [sub-rule](../sub-rule.md) 匹配；如果名称不存在或为空，则回退到主 `rules`
