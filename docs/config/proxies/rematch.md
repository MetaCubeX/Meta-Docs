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

## 使用示例

### 标记后继续匹配主规则

```{.yaml linenums="1"}
proxies:
- name: "mark-streaming"
  type: rematch
  target-rematch-name: "streaming"

proxy-groups:
- name: "Streaming"
  type: select
  proxies:
    - ss1
    - ss2

rules:
  - REMATCH-NAME,streaming,Streaming
  - DOMAIN-SUFFIX,netflix.com,mark-streaming
  - MATCH,DIRECT
```

当请求首次匹配 `DOMAIN-SUFFIX,netflix.com,mark-streaming` 时，`mark-streaming` 会写入 `rematch-name: streaming` 并重新进入规则匹配。重新匹配时会优先命中 `REMATCH-NAME,streaming,Streaming`。

!!! note
    `REMATCH-NAME` 规则应放在触发 rematch 的规则之前，避免重新匹配时再次命中同一个 rematch 出站形成循环。

### 跳转到子规则

```{.yaml linenums="1"}
proxies:
- name: "use-ai-rules"
  type: rematch
  target-sub-rule: "ai-rules"

proxy-groups:
- name: "AI"
  type: select
  proxies:
    - ss1
    - ss2

rules:
  - DOMAIN-SUFFIX,openai.com,use-ai-rules
  - MATCH,DIRECT

sub-rules:
  ai-rules:
    - DOMAIN-SUFFIX,openai.com,AI
    - MATCH,DIRECT
```

当请求匹配到 `use-ai-rules` 后，会使用 `ai-rules` 子规则继续匹配。子规则存在时建议设置 `MATCH` 兜底。
