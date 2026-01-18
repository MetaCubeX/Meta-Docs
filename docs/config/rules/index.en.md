# Routing Rules

```{.yaml linenums="1"}
rules:
- DOMAIN,ad.com,REJECT
- DOMAIN-SUFFIX,google.com,auto
- DOMAIN-KEYWORD,google,auto
- DOMAIN-WILDCARD,*.google.com,auto
- DOMAIN-REGEX,^abc.*com,PROXY
- GEOSITE,youtube,PROXY

- IP-CIDR,127.0.0.0/8,DIRECT,no-resolve
- IP-CIDR6,2620:0:2d0:200::7/32,auto
- IP-SUFFIX,8.8.8.8/24,PROXY
- IP-ASN,13335,DIRECT
- GEOIP,CN,DIRECT

- SRC-GEOIP,cn,DIRECT
- SRC-IP-ASN,9808,DIRECT
- SRC-IP-CIDR,192.168.1.201/32,DIRECT
- SRC-IP-SUFFIX,192.168.1.201/8,DIRECT

- DST-PORT,80,DIRECT
- SRC-PORT,7777,DIRECT

- IN-PORT,7890,PROXY
- IN-TYPE,SOCKS/HTTP,PROXY
- IN-USER,mihomo,PROXY
- IN-NAME,ss,PROXY

- PROCESS-PATH,/usr/bin/wget,PROXY
- PROCESS-PATH,C:\Program Files\Google\Chrome\Application\chrome.exe,PROXY
- PROCESS-PATH-WILDCARD,/usr/*/wget,PROXY
- PROCESS-PATH-REGEX,.*bin/wget,PROXY
- PROCESS-PATH-REGEX,(?i).*Application\\chrome.*,PROXY

- PROCESS-NAME,curl,PROXY
- PROCESS-NAME,chrome.exe,PROXY
- PROCESS-NAME,com.termux,PROXY
- PROCESS-NAME-WILDCARD,*telegram*,PROXY
- PROCESS-NAME-REGEX,curl$,PROXY
- PROCESS-NAME-REGEX,(?i)Telegram,PROXY
- PROCESS-NAME-REGEX,.*telegram.*,PROXY
- UID,1001,DIRECT

- NETWORK,udp,DIRECT
- DSCP,4,DIRECT

- RULE-SET,providername,proxy
- AND,((DOMAIN,baidu.com),(NETWORK,UDP)),DIRECT
- OR,((NETWORK,UDP),(DOMAIN,baidu.com)),REJECT
- NOT,((DOMAIN,baidu.com)),PROXY
- SUB-RULE,(NETWORK,tcp),sub-rule

- MATCH,auto
```

## Priority

Rules will be matched in order from top to bottom, with the rules at the top having higher priority than those below.

!!! warning ""
    If the request is for UDP and the proxy node does not support UDP (for example, if the `ss` node does not have `udp: true` specified), it will continue to match downwards.

### DOMAIN

Matches the full domain name.

### DOMAIN-SUFFIX

Matches the domain suffix.

For example, `google.com` matches `www.google.com`, `mail.google.com`, and `google.com`, but does not match `content-google.com`.

### DOMAIN-KEYWORD

Matches using domain keywords.

### DOMAIN-WILDCARD

Wildcard matching, only supports `*` and `?` wildcards.

Here `*` matches zero or more characters, `?` matches exactly one character.

!!! note
    Note that the wildcards here are different from the [Clash format wildcards](../../handbook/syntax.md#_8) elsewhere in the configuration file.

### DOMAIN-REGEX

Matches using regular expressions for domain names.

### GEOSITE

Matches domains within a Geosite; some content is referenced from [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data).

### IP-CIDR & IP-CIDR6

Matches IP address ranges; `IP-CIDR` and `IP-CIDR6` have the same effect, with `IP-CIDR6` being an alias.

### IP-SUFFIX

Matches IP suffix ranges.

### IP-ASN

Matches the ASN of the IP.

### GEOIP

Matches the country code of the IP.

### SRC-GEOIP

Matches the country code of the source IP.

### SRC-IP-ASN

Matches the ASN of the source IP.

### SRC-IP-CIDR

Matches the source IP address range.

### SRC-IP-SUFFIX

Matches the source IP suffix range.

### DST-PORT

Matches the target [port range](../../handbook/syntax.md#port-ranges) of the request.

### SRC-PORT

Matches the source [port range](../../handbook/syntax.md#port-ranges) of the request.

### IN-PORT

Matches the [inbound port](../inbound/listeners/index.md#port), allowing for [port ranges](../../handbook/syntax.md#port-ranges).

### IN-TYPE

Matches the [inbound type](../inbound/listeners/index.md#type).

### IN-USER

Matches the [inbound](../inbound/listeners/index.md) username, supporting multiple usernames separated by `/`.

### IN-NAME

Matches the [inbound name](../inbound/listeners/index.md#name).

### PROCESS-PATH

Matches using the full process path.

### PROCESS-PATH-WILDCARD

Process path wildcard matching is used, supporting only `*` and `?` wildcards.

Here `*` matches zero or more characters, `?` matches exactly one character.

!!! note
    Note that the wildcards here are different from the [Clash format wildcards](../../handbook/syntax.md#_8) elsewhere in the configuration file.

### PROCESS-PATH-REGEX

Matches using regular expressions for the process path.

### PROCESS-NAME

Matches using the process name; on the `Android` platform, it can match package names.

### PROCESS-NAME-WILDCARD

Uses process name wildcard matching, supporting only `*` and `?` wildcards. On the Android platform, it can also match package names.

Here `*` matches zero or more characters, `?` matches exactly one character.

!!! note
    Note that the wildcards here are different from the [Clash format wildcards](../../handbook/syntax.md#_8) elsewhere in the configuration file.

### PROCESS-NAME-REGEX

Matches using regular expressions for the process name; on the `Android` platform, it can match package names.

### UID

Matches the Linux USER ID.

### NETWORK

Matches `tcp` or `udp`.

### DSCP

Matches the `DSCP` tag (only for tproxy UDP inbound).

### RULE-SET

References a set of rules; configuration for [rule-providers](../rule-providers/index.md) is required.

### AND & OR & NOT

`LOGIC_TYPE,((payload1),(payload2)),Proxy`

*payload1* and *payload2* are rule types and other payloads, such as `DOMAIN,google.com`.

Logical rules require **careful use of parentheses**.

### SUB-RULE

Matches to [sub-rules](../sub-rule.md); careful use of parentheses is required.

### MATCH

Matches all requests without conditions.

## Additional Parameters

### no-resolve

Only supports rules regarding the `target IP`.

When domain matching begins for `target IP` rules, mihomo will trigger DNS resolution to check if the domain's `target IP` matches the rules. The `no-resolve` option can be selected to skip DNS resolution.

If DNS resolution was triggered in an earlier match, it will still match rules regarding `target IP` that have the `no-resolve` option added.

### src

Only supports rules regarding the `target IP`.

Converts `target IP` matching to `source IP` matching.
