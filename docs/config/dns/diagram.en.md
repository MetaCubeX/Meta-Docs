# Resolution Flow

## Example Configuration

```{.yaml linenums="1"}
dns:
  nameserver:
    - https://doh.pub/dns-query
  fallback:
    - https://8.8.8.8/dns-query
  direct-nameserver:
    - system
  nameserver-policy:
    "geosite:cn,private":
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'

rules:
- DOMAIN-SUFFIX,google.com,PROXY
- GEOIP,CN,DIRECT
- MATCH,PROXY
```

## Flow

!!! note ""
    This section only describes how the DNS module processes requests.

!!! warning ""
    ~~Re-resolution through `direct-nameserver` only applies to TCP connections.~~
    Since v1.19.10, re-resolution through `direct-nameserver` also applies to UDP connections. For TUN inbounds, this is limited to fake-IP mode.

```mermaid
flowchart TD
  Rule[Match rules]
  Domain[Matched a domain rule]
  IP[Matched a destination IP rule]
  Resolve[Resolve domain]

  NameServer[Query with nameserver]
  Policy[Match nameserver-policy]
  Concurrent[Query nameserver and fallback concurrently]
  Filter[Match fallback-filter]
  DirectNS[Re-resolve with direct-nameserver]

  GetIP[Resolved IP]

  Proxy[Send domain to proxy]
  Direct[Connect directly using IP]

  Rule --> Domain
  Rule --> IP

  Domain -- Domain matched DIRECT --> Resolve
  Domain -- Domain matched DIRECT and direct-nameserver is configured --> DirectNS

  IP --> Resolve

  Resolve -- nameserver-policy is configured --> Policy
  Policy -- No match --> NameServer
  Policy --> GetIP
  Resolve -- nameserver-policy is not configured --> NameServer

  NameServer -- fallback is configured --> Concurrent
  Concurrent --> Filter
  Filter --> GetIP
  NameServer -- fallback is not configured --> GetIP

  GetIP -- Matched DIRECT and direct-nameserver is configured --> DirectNS
  DirectNS --> Direct

  GetIP -- IP matched a proxy --> Proxy[Send domain to proxy]
  Domain -- Domain matched a proxy --> Proxy

  GetIP -- IP matched DIRECT --> Direct
```
