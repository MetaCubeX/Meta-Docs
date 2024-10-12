# DNS

```{.yaml linenums="1"}
dns:
  enable: true
  prefer-h3: false
  use-hosts: true
  use-system-hosts: true
  respect-rules: false
  listen: 0.0.0.0:1053
  ipv6: false
  default-nameserver:
    - 223.5.5.5
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter-mode: blacklist
  fake-ip-filter:
    - '*.lan'
  nameserver-policy:
    '+.arpa': '10.0.0.1'
    'rule-set:cn':
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  proxy-server-nameserver:
    - https://doh.pub/dns-query
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
```

## enable

Whether to enable; if set to false, the system DNS resolution will be used.

## prefer-h3

DOH will prioritize the use of HTTP/3.

## listen

DNS service listening, only supports UDP.

## IPV6

Whether to resolve IPV6; if set to false, it will respond with empty resolutions for AAAA records.

## enhanced-mode

Optional values are `fake-ip`/`redir-host`, default is `redir-host`.

This refers to the DNS processing mode of mihomo.

## fake-ip-range

Settings for the IP range under fakeip; the default IPV4 address for [tun](../inbound/tun.md) also uses this value as a reference.

## fake-ip-filter

Fakeip filtering; the following addresses will not receive fakeip mappings for connections.

Values support [domain wildcards](../../handbook/syntax.md#domain-wildcards) and [importing domain sets](../../handbook/syntax.md#introducing-domain-sets).

## fake-ip-filter-mode: blacklist

Optional values are `blacklist`/`whitelist`, default is `blacklist`. In `whitelist`, only successful matches will return fake-ip.

## use-hosts

Whether to respond to the configured [hosts](./hosts.md); default is true.

## use-system-hosts

Whether to query the system hosts; default is true.

## respect-rules

DNS connections comply with [routing rules](../rules/index.md), requiring configuration of [proxy-server-nameserver](./index.md#proxy-server-nameserver).

## default-nameserver

Default DNS, used for resolving the domain names of DNS servers.

!!! node ""
    Must be an IP, can be encrypted DNS.

## nameserver-policy

Specifies the resolving servers for domain queries, can use geosite, prioritized over `nameserver/fallback queries`.

Keys support [domain wildcards](../../handbook/syntax.md#domain-wildcards).

Values support strings/arrays.

## proxy-server-nameserver

The proxy node domain resolution server is used solely for resolving the domain names of proxy nodes. If left blank, it will follow the configurations of nameserver-policy, nameserver, and fallback.

## direct-nameserver

The DNS server for domain resolution at the direct exit. If left blank, it will follow the configurations of nameserver-policy, nameserver, and fallback.

## direct-nameserver-follow-policy

Indicates whether to adhere to the nameserver-policy. The default is not to comply, and it only takes effect when direct-nameserver is not empty.

## nameserver

Default domain name resolution server.

## fallback

Backup domain name resolution servers, generally using overseas DNS to ensure reliable results.

After configuring `fallback`, `fallback-filter` is enabled by default, with `geoip-code` set to CN.

## fallback-filter

Filtering for backup domain name resolution servers; those meeting the conditions will use `fallback` results or only use `fallback` for resolution.

### geoip

Whether to enable geoip judgment.

### geoip-code

Optional values are country abbreviations, default value is `CN`.

Results from IPs other than those configured with `geoip-code` will be considered polluted.

Results from the country configured with `geoip-code` will be adopted directly; otherwise, `fallback` results will be used.

### geosite

Optional values are collections included in the corresponding geosite.

Contents of the geosite list are considered polluted; domain names matching the geosite will only use `fallback` resolution, not `nameserver`.

### ipcidr

Written as `IP/mask`.

Results from these subnets will be considered polluted; when `nameserver` resolves these results, it will adopt `fallback` resolution results.

### domain

These domains are considered polluted; matching these domains will directly use `fallback` resolution, not `nameserver`.

## Additional Parameters

This section can be used to send DNS queries to public address DNS servers, using `#` to append and `&` to connect different parameters.

### DNS specifies proxy/interface for connection

Prioritize existing proxies; if a proxy with that name does not exist, specify the interface for connection.

`#RULES` is for connecting according to routing rules, equivalent to [respect-rules](./index.md#respect-rules).

If queries need to go through a proxy, configure `proxy-server-nameserver` to avoid issues.

```{.yaml linenums="1"}
nameserver:
  - 'tls://dns.google#proxy'
  - 'tls://dns.alidns.com#eth0'
```

### Force HTTP/3

This option does not conflict with `prefer-h3`; when filled in, it forces the use of HTTP/3 to establish DOH connections. Ensure that the DOH server supports HTTP/3 before use.

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#h3=true'
```

### Skip certificate verification for DOH

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#skip-cert-verify=true'
```

### ecs

Specify the subnet address for DNS queries, only supports [doh](./type.md#dns-over-https).

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24'
```

### ecs-override

Force override of the subnet address for DNS queries, only supports [doh](./type.md#dns-over-https).

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24&ecs-override=true'
```