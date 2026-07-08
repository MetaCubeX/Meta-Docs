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

## Примеры использования

### Пометить и продолжить сопоставление основными правилами

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

Когда запрос сначала совпадает с `DOMAIN-SUFFIX,netflix.com,mark-streaming`, `mark-streaming` записывает `rematch-name: streaming` и снова запускает сопоставление правил. На следующем проходе первым сработает `REMATCH-NAME,streaming,Streaming`.

!!! note
    Размещайте правило `REMATCH-NAME` перед правилом, которое запускает rematch, иначе следующий проход может снова попасть в тот же rematch outbound и создать цикл.

### Перейти к sub-rule

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

После совпадения с `use-ai-rules` сопоставление продолжается в sub-rule `ai-rules`. Если sub-rule существует, рекомендуется добавить fallback через `MATCH`.
