# General configuration

## Allow LAN

Allows other devices to access the internet through Clash [proxy port](./inbound/port.md).

Optional values: `true/false`

```{.yaml linenums="1"}
allow-lan: true
```

Binding address, only allows other devices to access through this address.

- `"*"` binds to all IP addresses.
- `"192.168.31.31"` binds to a single IPV4 address.
- `"[aaaa::a8aa:ff:fe09:57d8]"` binds to a single IPV6 address.

```{.yaml linenums="1"}
bind-address: "*"
```

Allowed IP address ranges for connection, applicable only when `allow-lan` is set to `true`.
Default values are `0.0.0.0/0` and `::/0`.

```{.yaml linenums="1"}
lan-allowed-ips:
- 0.0.0.0/0
- ::/0
```

Disallowed IP address ranges for connection. Blacklist takes precedence over whitelist, default is empty.

```{.yaml linenums="1"}
lan-disallowed-ips:
- 192.168.0.3/32
```

### User Authentication

User authentication for http(s), socks, and mixed proxies.

```{.yaml linenums="1"}
authentication:
- "user1:pass1"
- "user2:pass2"
```

Set the IP ranges allowed to skip authentication.

```{.yaml linenums="1"}
skip-auth-prefixes:
- 127.0.0.1/8
- ::1/128
```

## Operation Mode

- `rule` Rule-based matching
- `global` Global proxy (requires selecting proxy/strategy in GLOBAL proxy group)
- `direct` Global direct connection

defaulting to `rule` mode.

```{.yaml linenums="1"}
mode: rule
```

## Log Level

Controls the logging level of Clash core, only output to console and control page.

```{.yaml linenums="1"}
log-level: info
```

- `silent` Silent, no output.
- `error` Outputs logs of errors and unusable logs.
- `warning` Outputs logs of errors that do not affect operations, and logs of error level.
- `info` Outputs general operational logs, as well as logs of error and warning levels.
- `debug` Outputs as much information as possible during runtime.

## IPv6

Whether to allow the kernel to accept IPv6 traffic.

default is `true`.

```{.yaml linenums="1"}
ipv6: true
```

## TCP Keep Alive Interval

Controls the interval at which Clash sends out TCP Keep Alive packets to reduce temporary measures for mobile device power consumption.

unit is seconds

```{.yaml linenums="1"}
keep-alive-interval: 30
```

The time Clash discovers and closes an invalid TCP connection:

1 × keep-alive-interval + 9 × keep-alive-interval

## Process Matching Mode

Controls whether Clash matches processes.

- `always` Enables, forces matching of all processes.
- `strict` Default, Clash determines whether to enable.
- `off` Does not match processes, recommended for use on routers.

```{.yaml linenums="1"}
find-process-mode: strict
```

## External Control (API)

External controller, allows controlling your Clash kernel using RESTful API.

API listening address, you can change `127.0.0.1` to `0.0.0.0` to listen on all IPs.

```{.yaml linenums="1"}
external-controller: 127.0.0.1:9090
```

Unix socket API listening address

!!! warning ""
    Accessing API endpoints via Unix socket does not verify secrets. If enabled, please ensure security measures are in place.

```{.yaml linenums="1"}
external-controller-unix: mihomo.sock
```

HTTPS-API listening address, requires configuring the tls section for certificate and private key configuration, external-controller must also be filled in.

```{.yaml linenums="1"}
external-controller-tls: 127.0.0.1:9443
```

Access key for the API.

```{.yaml linenums="1"}
secret: ""
```

## External User Interface

Allows running static webpage resources (such as Clash-dashboard) on Clash API, path is API address/ui.

```{.yaml linenums="1"}
external-ui: /path/to/ui/folder
```

Can be an absolute path or a relative path to the Clash working directory.

## Custom External User Interface Name

```{.yaml linenums="1"}
external-ui-name: xd      # Merged into external-ui/xd
```

Not mandatory, will be updated to the specified folder during updates, if not configured, it will be updated directly to the `external-ui` directory.

## Custom External User Interface Download URL

```{.yaml linenums="1"}
external-ui-url: "<https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip>" # Get from GitHub Pages branch
```

## Cache

In Clash official, profile should be an extension configuration, but in Clash.meta, it is only used as a cache item.

```{.yaml linenums="1"}
profile:
  store-selected: true

# Stores API selections for strategy groups for use on the next start

  store-fake-ip: true

# Stores the fakeip mapping table, using the original mapping address when the domain connects again
```

## Unified Delay

Change delay calculation method, remove additional delays such as handshakes.

```{.yaml linenums="1"}
unified-delay: true
```

## TCP Concurrency

```{.yaml linenums="1"}
tcp-concurrent: true
```

## Outbound Interface

Clash's traffic outbound interface.

```{.yaml linenums="1"}
interface-name: en0
```

## Routing Mark

Provides a default traffic mark for outbound connections on Linux.

```{.yaml linenums="1"}
routing-mark: 6666
```

## TLS

Currently only used for https in API.

```{.yaml linenums="1"}
tls:
  certificate: string # Certificate PEM format or certificate path
  private-key: string # Private key PEM format corresponding to the certificate, or private key path
```

## Global Client Fingerprint

Global TLS fingerprint, lower priority than client-fingerprint inside proxy.

Currently supports TCP/grpc/WS/HTTP transport with TLS, supported protocols are `VLESS`, `Vmess`, and `trojan`.

```{.yaml linenums="1"}
global-client-fingerprint: chrome
```

!!! note
    Options: `chrome`, `firefox`, `safari`, `iOS`, `android`, `edge`, `360`, `qq`, `random`
    If `random` is selected, a modern browser fingerprint will be generated based on Cloudflare Radar data.

## GEO Data Mode

Change the geoip usage file, `mmdb` or `dat`,`true` is `dat`, with a default value of `false`.

```{.yaml linenums="1"}
geodata-mode: true
```

## GEO File Loading Mode

Optional loading modes are as follows:

- `standard`: Standard loader
- `memconservative`: Loader optimized for memory-limited (small memory) devices (default)

```{.yaml linenums="1"}
geodata-loader: memconservative
```

## Auto Update GEO

```{.yaml linenums="1"}
geo-auto-update: false
```

Update interval, unit is hours

```{.yaml linenums="1"}
geo-update-interval: 24
```

## Custom GEO Download Address

```{.yaml linenums="1"}
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

## Custom Global UA

Custom UA used when downloading external resources, default is clash.meta.

```{.yaml linenums="1"}
global-ua: clash.meta
```
