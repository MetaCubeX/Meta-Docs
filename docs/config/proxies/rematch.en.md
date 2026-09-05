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

## Usage Examples

### Mark and Continue Matching Main Rules

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

When a request first matches `DOMAIN-SUFFIX,netflix.com,mark-streaming`, `mark-streaming` writes `rematch-name: streaming` and enters rule matching again. On the next pass, `REMATCH-NAME,streaming,Streaming` matches first.

!!! note
    Put the `REMATCH-NAME` rule before the rule that triggers rematch, otherwise the next pass can hit the same rematch outbound again and form a cycle.

### Jump to a Sub-Rule

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

After a request matches `use-ai-rules`, matching continues with the `ai-rules` sub-rule. When the sub-rule exists, it is recommended to include a `MATCH` fallback.
