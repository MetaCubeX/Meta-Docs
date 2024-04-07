# Syntax

mihomo uses `yaml` as the configuration file format.

`yaml` is case-sensitive and uses indentation to represent hierarchical relationships. Tabs are not allowed for indentation; only spaces are permitted. The number of spaces for indentation is not critical, as long as elements at the same level are left-aligned.

## Comments

In a `yaml` file, comments start with "#" at the beginning of a line and end with the line. "#" must be at the beginning of a line or must have a space in front to be considered a comment.

```{.yaml linenums="1"}
port: 7890 # http 代理端口
socks-port: 7891
# socks 代理端口
```

## Objects

Key-value pairs in objects are represented using a colon structure key: value. There should be a space after the colon, and indentation is used to show hierarchical relationships.

As yaml is a superset of json, you can directly use json syntax.

### Multiline

```{.yaml linenums="1"}
tun:
  enable: true
  stack: system
  auto-route: true
  auto-detect-interface: true
```

### Multiline JSON Extension

```{.yaml linenums="1"}
tun: { 
  enable: true,
  stack: system,
  auto-route: true,
  auto-detect-interface: true
  }
```

### Single-line JSON Extension

```{.yaml linenums="1"}
tun: { enable: true, stack: system, auto-route: true, auto-detect-interface: true}
```

### Full JSON (Not Recommended)

```{.json linenums="1"}
{
  "tun": {
    "enable": true,
    "stack": "system",
    "auto-route": true,
    "auto-detect-interface": true
  }
}
```

## Arrays

Lines starting with - form an array and are used for multiple values within an object.

### Multiline Arrays

```{.yaml linenums="1"}
a:
  - b
  - c
  - d
```

### Single-line JSON Arrays

```{.yaml linenums="1"}
a: [b, c, d]
```

## References

`&` for anchors and `*` for aliases can be used for referencing. `&` is used to establish an anchor, `<<` is used to merge into the current data, and `*` is used to reference an anchor.

In the following example, because the key p does not exist in Clash, it will be ignored when Clash parses the configuration. If there are duplicate keys during merging, the merging process will not occur.

```{.yaml linenums="1"}
p: &p
  type: http
  interval: 3600
  health-check:
    enable: true
    url: https://www.gstatic.com/generate_204
    interval: 300

proxy-providers:
  provider1:
    <<: *p
    url: ""
    path: ./proxy_providers/provider1.yaml

  provider2:
    <<: *p
    type: file
    path: ./proxy_providers/provider2.yaml
```

Equivalent to:

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
    url: ""
    path: ./proxy_providers/provider1.yaml

  provider2:
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
    type: file
    path: ./proxy_providers/provider2.yaml
```

## IPv6 Addresses

In Clash configuration, use `[]` to frame an IPv6 address.

```{.yaml linenums="1"}
[aaaa::a8aa:ff:fe09:57d8]
[aaaa::a8aa:ff:fe09:57d9]:853 # 带端口的 IPV6 地址
```

## Domain Wildcards

### Wildcard `*`

Clash's `*` wildcard can only match one-level domains.

`*.baidu.com` only matches `tieba.baidu.com` and not `123.tieba.baidu.com` or `baidu.com`.

A single `'*'` only matches domains like `localhost`.

### Wildcard `+`

The `+` wildcard is similar to DOMAIN-SUFFIX and can match multiple levels at once.

`+.baidu.com` matches `tieba.baidu.com` and `123.tieba.baidu.com` or `baidu.com`. The `+` wildcard can only be used for domain prefix matching.

### Wildcard `.`

The `.` wildcard can match multiple levels at once.
`.baidu.com` matches `tieba.baidu.com` and `123.tieba.baidu.com` but not `baidu.com`. The `.` wildcard can only be used for domain prefix matching.

### Usage Example

When using wildcards, be sure to wrap the content in single or double quotes (`''` or `""`) to prevent excessive matching.

```{.yaml linenums="1"}
fake-ip-filter:
  - ".lan"
  - "xbox.*.microsoft.com"
  - "+.xboxlive.com"
  - localhost.ptlogin2.qq.com
```

<!-- 
## Time Format

Mihome supports two time formats: integer and duration.

=== "Integer format"
    ```{.yaml linenums="1"}
    interval: 3600
    ```

=== "Duration format"
    ```{.yaml linenums="1"}
    interval: 1h
    ```
-->
