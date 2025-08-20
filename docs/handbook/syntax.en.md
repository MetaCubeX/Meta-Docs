# Grammar

Mihomo uses `yaml` as the configuration file format.

`yaml` is case-sensitive and uses indentation to indicate hierarchical relationships. Tabs are not allowed for indentation; only spaces are permitted. The number of spaces used for indentation is not important, as long as elements at the same level are left-aligned.

## Comments

In `yaml` formatted files, comments start with "#" and extend to the end of the line. The "#" must be at the beginning of the line or preceded by a space; otherwise, it is not considered a comment.

```{.yaml linenums="1"}
port: 7890 # HTTP proxy port
socks-port: 7891
# SOCKS proxy port
```

## Objects

Key-value pairs in objects are represented using the colon structure `key: value`, with a space required after the colon. Indentation indicates hierarchical relationships.

Since the `yaml` format is a superset of `json`, you can directly use `json` syntax.

### Multi-line

```{.yaml linenums="1"}
tun:
  enable: true
  stack: system
  auto-route: true
  auto-detect-interface: true
```

### Multi-line JSON

```{.yaml linenums="1"}
tun: { 
  enable: true,
  stack: system,
  auto-route: true,
  auto-detect-interface: true
}
```

### Single-line JSON

```{.yaml linenums="1"}
tun: { enable: true, stack: system, auto-route: true, auto-detect-interface: true }
```

### Full JSON

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

Lines starting with `-` indicate the formation of an array, used for multiple values within an object.

### Multi-line Array

```{.yaml linenums="1"}
a:
  - b
  - c
  - d
```

### Single-line JSON Array

```{.yaml linenums="1"}
a: [b, c, d]
```

## References

`&` is used for anchors and `*` for aliases, allowing for references. `&` establishes an anchor, while `<<` indicates merging into the current data, and `*` is used to reference an anchor.

In the following example, since the key `p` does not exist in `mihomo`, it will be ignored during configuration parsing.

If there are duplicate items during merging, they will not be merged.

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

This is equivalent to:

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

## Domain Wildcards

!!! note
    The domain wildcard in this section is not the same as [`DOMAIN-WILDCARD` in the routing rules](../config/rules/#domain-wildcard).

### Wildcard `*`

Clash's wildcard `*` can only match one level of domain at a time.

`*.baidu.com` matches `tieba.baidu.com` but does not match `123.tieba.baidu.com` or `baidu.com`.

`*` only matches hostnames without a `.` such as localhost.

### Wildcard `+`

The wildcard `+` is similar to DOMAIN-SUFFIX and can match multiple levels at once.

`+.baidu.com` matches `tieba.baidu.com`, `123.tieba.baidu.com`, and `baidu.com`.

The wildcard `+` can only be used for prefix matching of domain names.

### Wildcard `.`

The wildcard `.` can match multiple levels at once.

`.baidu.com` matches `tieba.baidu.com` and `123.tieba.baidu.com`, but does not match `baidu.com`.

The wildcard `.` can only be used for prefix matching of domain names.

### Usage Example

When using wildcards, you should wrap the content in quotes `' '` or `" "`.

```{.yaml linenums="1"}
fake-ip-filter:
- ".lan"
- "xbox.*.microsoft.com"
- "+.xboxlive.com"
- localhost.ptlogin2.qq.com
```

## Introducing Domain Sets

!!! warning
    The rule-set only supports behavior as domain/classical.

```{.yaml linenums="1"}
fake-ip-filter:
- "rule-set:xxx"
- "geosite:xxx"
```

## Port Ranges

Mihomo can use `-` to match port ranges, and `/` or `,` to separate multiple ports/port ranges.

### Example

Match ports 114 to 514 and 810 to 1919, as well as port 65530.

```{.yaml linenums="1"}
114-514/810-1919,65530
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
