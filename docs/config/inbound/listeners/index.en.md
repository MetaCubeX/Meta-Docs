# LISTENERS

```{.yaml linenums="1"}
listeners:
- name: in-name
  type: shadowsocks
  port: 10000
  listen: 0.0.0.0
  routing-mark: 0
  rule: sub-rule-1
  proxy: proxy
```

## name

Inbound name. It can be matched by an [`inbound rule`](../../rules/index.md#in-name).

## type

Inbound type.

## listen

Listening address.

## port

Listening port. Supports the ports format described in [Port ranges](../../../handbook/syntax.md#port-ranges).

## routing-mark

Sets the routing mark for the listening socket. Linux only.

## rule

Uses a [`sub-rule`](../../sub-rule.md) as the matching rules for this inbound. When empty, [`rules`](../../rules/index.md) is used by default. If the [`sub-rule`](../../sub-rule.md) is not found, [`rules`](../../rules/index.md) is used directly.

## proxy

Uses an [`outbound proxy`](../../proxies/index.md) or [`proxy group`](../../proxy-groups/index.md) directly. When empty, [`rules`](../../rules/index.md) is used by default.

!!! warning ""
    When `proxy` is non-empty, its value must be a valid proxy name or an error will occur.
