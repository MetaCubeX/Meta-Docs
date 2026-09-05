---
hide:
  - navigation
#   - toc
---

# API

## Request Example

Curl example: `curl -H 'Authorization: Bearer ${secret}' http://${controller-api}/configs?force=true -d '{"path": "", "payload": ""}' -X PUT`

This request includes the header `'Authorization: Bearer ${secret}'`, where:

- `${secret}` is the API key set in the configuration file [api](../config/general.md#external-control-api)
- `${controller-api}` is the API listening address set in the configuration file [api](../config/general.md#external-control-api)
- `?force=true` is a parameter that needs to be included in certain requests
- `'{"path": "", "payload": ""}'` is the data for the resource to be updated

!!! note
    If you need to pass a path, please note that if the path is not in the mihomo working directory, please manually set the `SAFE_PATHS` environment variable to add it to the safe path. The syntax of this environment variable is the same as the PATH environment variable parsing rules of this operating system (i.e., semicolon-separated in Windows and colon-separated in other systems).

## Logs

### `/logs`

!!! info ""
    Retrieve real-time logs

- Request method: `GET` / `WS`
- Optional parameter: `?level=log_level`, where `log_level` can be `info`, `warning`, `error`, `debug`
- Optional parameter: `?format=structured`, when present outputs structured logs (with `time`, `level`, `message`, `fields` fields)
- Response fields (standard mode, one JSON per line):
    - `type`: log level — `info` / `warning` / `error` / `debug`
    - `payload`: log message content
- Response fields (structured mode `?format=structured`):
    - `time`: time string in `HH:MM:SS` format
    - `level`: log level
    - `message`: log message content
    - `fields`: array of additional fields

## Traffic Information

### `/traffic`

!!! info ""
    Retrieve real-time traffic; `up`/`down` are in bytes/s, `upTotal`/`downTotal` are in bytes

- Request method: `GET` / `WS`
- Response fields (pushed once per second):
    - `up`: current upload rate (bytes/s)
    - `down`: current download rate (bytes/s)
    - `upTotal`: cumulative uploaded bytes
    - `downTotal`: cumulative downloaded bytes

## Memory Information

### `/memory`

!!! info ""
    Retrieve real-time memory usage, measured in bytes

- Request method: `GET` / `WS`
- Response fields (pushed once per second):
    - `inuse`: current memory in use (bytes)
    - `oslimit`: OS memory limit (bytes, always `0`)

## Version Information

### `/version`

!!! info ""
    Retrieve the mihomo version

- Request method: `GET`
- Response fields:
    - `meta`: whether this is a Meta build (`true` / `false`)
    - `version`: version string

## Cache

### `/cache/fakeip/flush`

!!! info ""
    Clear the fake IP cache

- Request method: `POST`
- Response: none (HTTP 204)

### `/cache/dns/flush`

!!! info ""
    Clear the DNS cache

- Request method: `POST`
- Response: none (HTTP 204)

## Running Configuration

### `/configs`

!!! info ""
    Retrieve basic configuration

- Request method: `GET`
- Response fields: JSON object of the current running configuration, including `port`, `socks-port`, `mixed-port`, `mode`, `log-level`, `allow-lan`, `ipv6`, `tun`, and other fields

!!! info ""
    Reload basic configuration

- Request method: `PUT`
- Parameter: `?force=true`
- Response: none (HTTP 204)

!!! info ""
    Update basic configuration

- Request method: `PATCH`
- Data: `'{"mixed-port": 7890}'`
- Response: none (HTTP 204)

### `/configs/geo`

!!! info ""
    Update the GEO database

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`
- Response: none (HTTP 204)

### `/restart`

!!! info ""
    Restart the kernel

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`
- Response: none (HTTP 204)

## Updates

### `/upgrade`

!!! info ""
    Update the kernel

- Request method: `POST`
- Optional parameters: `?channel=xxx` specifies the update channel, `?force=true` forces the update
- Data: `'{"path": "", "payload": ""}'`
- Response fields:
    - `status`: fixed value `"ok"`

### `/upgrade/ui`

!!! info ""
    Update the panel; [external-ui](../config/general.md#external-user-interface) must be set

- Request method: `POST`
- Response fields:
    - `status`: fixed value `"ok"`

### `/upgrade/geo`

!!! info ""
    Update the GEO database

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`
- Response: none (HTTP 204)

## Policy Groups

### `/group`

!!! info ""
    Retrieve policy group information

- Request method: `GET`
- Response fields:
    - `proxies`: array of policy group objects, each in the same format as `/proxies/proxies_name`

### `/group/group_name`

!!! info ""
    Retrieve specific policy group information

- Request method: `GET`
- Response fields: policy group object, same format as `/proxies/proxies_name`

### `/group/group_name/delay`

!!! info ""
    Test the nodes/strategy groups within the specified strategy group, return new latency information, and clear the fixed selection of the automatic strategy group

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`
- Optional parameter: `?expected=xxx`, the expected HTTP response status code, supports ranges (e.g. `200/204`, `200-299`)
- Response fields: JSON object mapping node names to their latency in milliseconds (`uint16`), e.g. `{"Node A": 120, "Node B": 350}`

## Proxies

### `/proxies`

!!! info ""
    Retrieve proxy information

- Request method: `GET`
- Response fields:
    - `proxies`: object keyed by proxy/group name; every entry shares these **common fields**:
        - `name`: proxy or group name
        - `type`: type (e.g. `Shadowsocks`, `VMess`, `DIRECT`, `Selector`, `URLTest`, `Fallback`, `LoadBalance`, etc.)
        - `udp`: whether UDP is supported
        - `uot`: whether UDP over TCP is supported
        - `xudp`: whether XUDP is supported
        - `tfo`: whether TCP Fast Open is enabled
        - `mptcp`: whether MPTCP is enabled
        - `smux`: whether stream multiplexing is enabled
        - `alive`: whether the proxy is currently alive
        - `history`: array of delay history entries, each with `time` and `delay` (ms)
        - `extra`: extra delay histories grouped by test URL
        - `interface`: bound network interface name
        - `routing-mark`: routing mark value
        - `provider-name`: name of the proxy provider this proxy belongs to
        - `dialer-proxy`: underlying dialer proxy name
    - **Policy groups** (`Selector`, `URLTest`, `Fallback`, `LoadBalance`) additionally include:
        - `now`: currently selected node name (absent for `LoadBalance`)
        - `all`: array of all node/group names in the group
        - `testUrl`: health check URL
        - `hidden`: whether the group is hidden in the dashboard
        - `icon`: icon URL
        - `emptyFallback`: fallback node name used when all members are unavailable
        - `expectedStatus`: expected HTTP response status code for health checks (absent for `Selector`)
        - `fixed`: currently pinned node name (only `URLTest` and `Fallback`)

### `/proxies/proxies_name`

!!! info ""
    Retrieve specific proxy information

- Request method: `GET`
- Response fields: proxy or group object, same fields as a single entry under `/proxies` (regular proxies have only the common fields; policy groups additionally include `now`, `all`, etc.)

!!! info ""
    Select a specific proxy

- Request method: `PUT`
- Data: `'{"name":"Japan"}'`
- Response: none (HTTP 204)

!!! info ""
    Clear the fixed selection of the proxy/policy group (except for the `Selector` type)

- Request method: `DELETE`
- Response: none (HTTP 204)

### `/proxies/proxies_name/delay`

!!! info ""
    Test a specified proxy and return new delay information

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`
- Optional parameter: `?expected=xxx`, the expected HTTP response status code, supports ranges (e.g. `200/204`, `200-299`)
- Response fields:
    - `delay`: measured latency in milliseconds (`uint16`)

## Proxy Sets

### `/providers/proxies`

!!! info ""
    Retrieve all information for all proxy sets

- Request method: `GET`
- Response fields:
    - `providers`: object keyed by provider name, each containing provider metadata and its proxy list

### `/providers/proxies/providers_name`

!!! info ""
    Retrieve information for a specific proxy set

- Request method: `GET`
- Response fields: proxy provider object including configuration metadata and a `proxies` list

!!! info ""
    Update the proxy set

- Request method: `PUT`
- Response: none (HTTP 204)

### `/providers/proxies/providers_name/healthcheck`

!!! info ""
    Trigger a health check for a specific proxy set

- Request method: `GET`
- Response: none (HTTP 204)

### `/providers/proxies/providers_name/proxies_name`

!!! info ""
    Retrieve information for a specified proxy within the proxy set

- Request method: `GET`
- Response fields: proxy object, same fields as `/proxies/proxies_name`

### `/providers/proxies/providers_name/proxies_name/healthcheck`

!!! info ""
    Test a specified proxy within the proxy set and return new delay information

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`
- Response fields:
    - `delay`: measured latency in milliseconds (`uint16`)

## Rules

### `/rules`

!!! info ""
    Retrieve rule information

- Request method: `GET`
- Response fields:
    - `rules`: array of rule objects, each containing:
        - `index`: rule index (0-based)
        - `type`: rule type (e.g. `DOMAIN`, `IP-CIDR`, `GEOIP`, etc.)
        - `payload`: rule match content
        - `proxy`: target proxy or policy group name
        - `size`: number of entries in the rule set (only valid for `GEOIP` / `GEOSITE`, `-1` otherwise)
        - `extra` (optional, when present includes):
            - `disabled`: whether the rule is disabled
            - `hitCount`: number of times the rule was matched
            - `hitAt`: timestamp of the last match
            - `missCount`: number of times the rule was not matched
            - `missAt`: timestamp of the last miss

### `/rules/disable`

!!! info ""
    Disable rules, where the key is the rule index and the value is whether to disable the rule. This is a temporary operation and is reset after a restart.

- Request method: `PATCH`
- Data: `'{"0": false,"1": true}'`
- Response: none (HTTP 204)

## Rule Sets

### `/providers/rules`

!!! info ""
    Retrieve all information for all rule sets

- Request method: `GET`
- Response fields:
    - `providers`: object keyed by rule provider name

### `/providers/rules/providers_name`

!!! info ""
    Update the rule set

- Request method: `PUT`
- Response: none (HTTP 204)

## Connections

### `/connections`

!!! info ""
    Retrieve connection information

- Request method: `GET` / `WS`
- Optional parameter: `?interval=milliseconds`, where `milliseconds` is the refresh interval, default value is 1000 milliseconds
- Response fields:
    - `downloadTotal`: cumulative downloaded bytes
    - `uploadTotal`: cumulative uploaded bytes
    - `memory`: current memory usage (bytes)
    - `connections`: array of active connection objects, each containing:
        - `id`: unique connection ID
        - `metadata`: connection metadata (source/destination address, protocol, process name, etc.)
        - `upload`: bytes uploaded on this connection
        - `download`: bytes downloaded on this connection
        - `start`: connection start time
        - `chains`: proxy chain array
        - `providerChains`: provider proxy chain array
        - `rule`: matched rule type
        - `rulePayload`: matched rule content

!!! info ""
    Close all connections

- Request method: `DELETE`
- Response: none (HTTP 204)

### `/connections/:id`

!!! info ""
    Close a specific connection

- Request method: `DELETE`
- Response: none (HTTP 204)

## Domain Query

### `/dns/query`

!!! info ""
    Retrieve DNS query data for a specified name and type

- Request method: `GET`
- Parameter: `?name=example.com&type=A`
- Response fields:
    - `Status`: DNS response code (Rcode)
    - `Question`: array of query questions
    - `TC`: whether the response is truncated
    - `RD`: recursion desired flag
    - `RA`: recursion available flag
    - `AD`: authenticated data flag
    - `CD`: checking disabled flag
    - `Answer` (optional): array of answer records, each with `name`, `type`, `TTL`, `data`
    - `Authority` (optional): array of authority records, same format as `Answer`
    - `Additional` (optional): array of additional records, same format as `Answer`

## Storage

### `/storage/key`

!!! info ""
    Retrieve the storage value for the specified key, returns `null` if it does not exist

- Request method: `GET`
- Response fields: any valid JSON value that was stored, or `null` if the key does not exist

!!! info ""
    Write the storage value for the specified key; the data must be valid JSON, maximum 1MB

- Request method: `PUT`
- Data: `'{"foo": "bar"}'`
- Response: none (HTTP 204)

!!! info ""
    Delete the storage value for the specified key

- Request method: `DELETE`
- Response: none (HTTP 204)

## DEBUG

`/debug` requires the kernel to be started with the [log level](../config/general.md#log-level) set to `debug`.

### `/debug/gc`

!!! info ""
    Perform active garbage collection

- Request method: `PUT`
- Response: none (HTTP 204)

### `/debug/pprof`

Open in a browser `http://${controller-api}/debug/pprof` to view raw DEBUG information, where:

- `allocs` indicates memory allocation for each function call, including the size of memory allocated on the stack and heap, as well as the number of allocations. This report is primarily to help identify memory leaks and frequent memory requests in the code.
- The `heap` report provides detailed information about memory usage on the heap, including the size, number, and address of allocated memory blocks, sorted by size. This report is mainly to locate areas of excessive memory usage; we can check object sizes in the heap report to find areas of high memory consumption.

#### Install [Graphviz](https://graphviz.org/download/) to view graphical debug information.

##### View Graphical Heap Report

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/heap
```

[Full image](../assets/image/api/heap.svg)

##### View Graphical Allocs Report

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/allocs
```

[Sample output](../assets/image/api/allocs.svg)

##### Submit Output Report

Access `http://${controller-api}/debug/pprof/heap?raw=true` in a browser to download this file, and upload it to [issues](https://github.com/MetaCubeX/mihomo/issues) to report any problems you encounter.
