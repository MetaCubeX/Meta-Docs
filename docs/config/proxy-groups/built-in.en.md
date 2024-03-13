# Preset outbound

## DIRECT

Direct, data exits directly.

## REJECT

Reject, intercepts data from going out.

## REJECT-DROP

Reject, unlike `REJECT`, this strategy silently drops the request.

## PASS

Bypass, skipping this rule when matching.

## COMPATIBLE

Compatible, appears when no node is selected in the policy group filtering, equivalent to `DIRECT`.
