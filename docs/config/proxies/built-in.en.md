# Preset outbound

## DIRECT

Direct, data exits directly.

## REJECT

Reject, intercepts data from going out.

## REJECT-DROP

Reject, unlike `REJECT`, this strategy silently drops the request.

## PASS

Bypass. When a rule resolves to `PASS`, it is treated as a matched outbound and the current matched rule branch is skipped, then matching continues with subsequent rules.

If `PASS` is matched inside `SUB-RULE`, matching leaves that sub-rule and continues in the main rules; subsequent rules in the same `sub-rules` list are not evaluated.

## COMPATIBLE

Compatible, appears when no node is selected in the policy group filtering, equivalent to `DIRECT`.
