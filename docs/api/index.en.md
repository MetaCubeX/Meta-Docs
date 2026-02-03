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

## Traffic Information

### `/traffic`

!!! info ""
    Retrieve real-time traffic, measured in kbps

- Request method: `GET` / `WS`

## Memory Information

### `/memory`

!!! info ""
    Retrieve real-time memory usage, measured in kb

- Request method: `GET` / `WS`

## Version Information

### `/version`

!!! info ""
    Retrieve the Clash version

- Request method: `GET`

## Cache

### `/cache/fakeip/flush`

!!! info ""
    Clear the fake IP cache

- Request method: `POST`

### `/cache/dns/flush`

!!! info ""
    Clear the DNS cache

- Request method: `POST`

## Running Configuration

### `/configs`

!!! info ""
    Retrieve basic configuration

- Request method: `GET`

!!! info ""
    Reload basic configuration

- Request method: `PUT`
- Parameter: `?force=true`

!!! info ""
    Update basic configuration

- Request method: `PATCH`
- Data: `'{"mixed-port": 7890}'`

### `/configs/geo`

!!! info ""
    Update the GEO database

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`

### `/restart`

!!! info ""
    Restart the kernel

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`

## Updates

### `/upgrade`

!!! info ""
    Update the kernel

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`

### `/upgrade/ui`

!!! info ""
    Update the panel; [external-ui](../config/general.md#external-user-interface) must be set

- Request method: `POST`

### `/upgrade/geo`

!!! info ""
    Update the GEO database

- Request method: `POST`
- Data: `'{"path": "", "payload": ""}'`

## Policy Groups

### `/group`

!!! info ""
    Retrieve policy group information

- Request method: `GET`

### `/group/group_name`

!!! info ""
    Retrieve specific policy group information

- Request method: `GET`

!!! info ""
    Clear the fixed selection of the automatic policy group

- Request method: `DELETE`

### `/group/group_name/delay`

!!! info ""
    Test the nodes/strategy groups within the specified strategy group, return new latency information, and clear the fixed selection of the automatic strategy group

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`

## Proxies

### `/proxies`

!!! info ""
    Retrieve proxy information

- Request method: `GET`

### `/proxies/proxies_name`

!!! info ""
    Retrieve specific proxy information

- Request method: `GET`

!!! info ""
    Select a specific proxy

- Request method: `PUT`
- Data: `'{"name":"Japan"}'`

### `/proxies/proxies_name/delay`

!!! info ""
    Test a specified proxy and return new delay information

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`

## Proxy Sets

### `/providers/proxies`

!!! info ""
    Retrieve all information for all proxy sets

- Request method: `GET`

### `/providers/proxies/providers_name`

!!! info ""
    Retrieve information for a specific proxy set

- Request method: `GET`

!!! info ""
    Update the proxy set

- Request method: `PUT`

### `/providers/proxies/providers_name/healthcheck`

!!! info ""
    Trigger a health check for a specific proxy set

- Request method: `GET`

### `/providers/proxies/providers_name/proxies_name/healthcheck`

!!! info ""
    Test a specified proxy within the proxy set and return new delay information

- Request method: `GET`
- Parameter: `?url=xxx&timeout=5000`

## Rules

### `/rules`

!!! info ""
    Retrieve rule information

- Request method: `GET`

## Rule Sets

### `/providers/rules`

!!! info ""
    Retrieve all information for all rule sets

- Request method: `GET`

### `/providers/rules/providers_name`

!!! info ""
    Update the rule set

- Request method: `PUT`

## Connections

### `/connections`

!!! info ""
    Retrieve connection information

- Request method: `GET` / `WS`
- Optional parameter: `?interval=milliseconds`, where `milliseconds` is the refresh interval, default value is 1000 milliseconds

!!! info ""
    Close all connections

- Request method: `DELETE`

### `/connections/:id`

!!! info ""
    Close a specific connection

- Request method: `DELETE`

## Domain Query

### `/dns/query`

!!! info ""
    Retrieve DNS query data for a specified name and type

- Request method: `GET`
- Parameter: `?name=example.com&type=A`

## DEBUG

`/debug` requires the kernel to be started with the [log level](../config/general.md#log-level) set to `debug`.

### `/debug/gc`

!!! info ""
    Perform active garbage collection

- Request method: `PUT`

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
