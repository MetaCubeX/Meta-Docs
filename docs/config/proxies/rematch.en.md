# Rematch

```{.yaml linenums="1"}
proxies:
- name: "rematch"
  type: rematch
  target-rematch-name: "rematch1"
  target-sub-rule: "sub-rule1"
```

[Common fields](./index.md)

## target-rematch-name

Overrides the original rematch-name in metadata. It can be matched with the [REMATCH-NAME](../rules/index.md#rematch-name) rule.

## target-sub-rule

If set, matching continues with the specified [sub-rule](../sub-rule.md). If the name does not exist or is empty, matching falls back to the main `rules`.
