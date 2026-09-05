# Domain Sniffing

```{.yaml linenums="1"}
sniffer:
  enable: false
  force-dns-mapping: true
  parse-pure-ip: true
  override-destination: false
  sniff:
    HTTP:
      ports: [80, 8080-8880]
      override-destination: true
    TLS:
      ports: [443, 8443]
    QUIC:
      ports: [443, 8443]
  force-domain:
    - +.v2ex.com
  skip-domain:
    - Mijia Cloud
  skip-src-address:
    - 192.168.0.3/32
  skip-dst-address:
    - 192.168.0.3/32
```

## enable

Whether to enable the sniffer.

## force-dns-mapping

Force sniffing for traffic identified through `redir-host` mappings.

## parse-pure-ip

Force sniffing for all traffic where no domain name was obtained.

## override-destination

Whether to use the sniffing result as the actual destination. Defaults to `true`.

## sniff

Protocols to sniff. Only `HTTP`, `TLS`, and `QUIC` are supported.

- `ports`: [Port range](../../handbook/syntax.md#port-ranges)
- `override-destination`: Overrides the global `override-destination` setting

## force-domain

List of domains that must be sniffed. Uses [domain wildcards](../../handbook/syntax.md#domain-wildcards).

## skip-domain

List of domains excluded from sniffing. Uses [domain wildcards](../../handbook/syntax.md#domain-wildcards).

## skip-src-address

List of source IP ranges excluded from sniffing.

## skip-dst-address

List of destination IP ranges excluded from sniffing.
