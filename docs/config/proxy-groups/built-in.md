# 预置出站

## DIRECT

直连，数据直接出站

## REJECT

拒绝，拦截数据出站

## REJECT-DROP

拒绝，与`REJECT`不同的是，该策略将静默抛弃请求

## PASS

绕过，会使匹配规则时跳过此规则

## COMPATIBLE

兼容，在策略组筛选不出节点时出现，等效 `DIRECT`
