# Rematch

```{.yaml linenums="1"}
proxies:
- name: "rematch"
  type: rematch
  target-rematch-name: "rematch1"
  target-sub-rule: "sub-rule1"
```

[Общие поля](./index.md)

## target-rematch-name

Переопределяет исходное rematch-name в metadata. Его можно сопоставлять правилом [REMATCH-NAME](../rules/index.md#rematch-name).

## target-sub-rule

Если задано, дальнейшее сопоставление выполняется указанным [sub-rule](../sub-rule.md). Если имя не существует или пустое, используется основной `rules`.
