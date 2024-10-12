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

In most cases, the data passed is `'{"path": "", "payload": ""}'`, which can include a new configuration file path.

## Logs

### `/logs`

Request method: `GET`

- Retrieve real-time logs.

## Traffic Information

### `/traffic`

Request method: `GET`

- Retrieve real-time traffic, measured in kbps.

## Memory Information

### `/memory`

Request method: `GET`

- Retrieve real-time memory usage, measured in kb.

## Version Information

### `/version`

Request method: `GET`

- Retrieve the Clash version.

## Cache

### `/cache/fakeip/flush`

Request method: `POST`

- Clear the fake IP cache.

## Running Configuration

### `/configs`

Request method: `GET`

- Retrieve basic configuration.

Request method: `PUT`

- Reload basic configuration; data must be sent, and the URL must include `?force=true` to enforce execution.

Request method: `PATCH`

- Update basic configuration; data must be sent in the format `'{"mixed-port": 7890}'`, modified as needed for the configuration items to be updated.

### `/configs/geo`

Request method: `POST`

- Update the GEO database; data must be sent.

### `/restart`

Request method: `POST`

- Restart the kernel; data must be sent.

## Updates

### `/upgrade`

Request method: `POST`

- Update the kernel; data must be sent.

### `/upgrade/ui`

Request method: `POST`

- Update the panel; [external-ui](../config/general.md#external-user-interface) must be set.

### `/upgrade/geo`

Request method: `POST`

- Update the GEO database; data must be sent.

## Policy Groups

### `/group`

Request method: `GET`

- Retrieve policy group information.

### `/group/group_name`

Request method: `GET`

- Retrieve specific policy group information.

Request method: `DELETE`

- Clear the fixed selection of the automatic policy group.

### `/group/group_name/delay`

Request method: `GET`

- Test the nodes/strategy groups within the specified strategy group, return new latency information, and clear the fixed selection of the automatic strategy group
- the URL must include `?url=xxx&timeout=5000`, modified as needed.

## Proxies

### `/proxies`

Request method: `GET`

- Retrieve proxy information.

### `/proxies/proxies_name`

Request method: `GET`

- Retrieve specific proxy information.

Request method: `PUT`

- Select a specific proxy; data must be included in the format `'{"name":"Japan"}'`.

### `/proxies/proxies_name/delay`

Request method: `GET`

- Test a specified proxy and return new delay information
- the URL must include `?url=xxx&timeout=5000`, modified as needed.

## Proxy Sets

### `/providers/proxies`

Request method: `GET`

- Retrieve all information for all proxy sets.

### `/providers/proxies/providers_name`

Request method: `GET`

- Retrieve information for a specific proxy set.

Request method: `PUT`

- Update the proxy set.

### `/providers/proxies/providers_name/healthcheck`

Request method: `GET`

- Trigger a health check for a specific proxy set.

### `/providers/proxies/providers_name/proxies_name/healthcheck`

- Test a specified proxy within the proxy set and return new delay information
- the URL must include `?url=xxx&timeout=5000`, modified as needed.

## Rules

### `/rules`

Request method: `GET`

- Retrieve rule information.

## Rule Sets

### `/providers/rules`

Request method: `GET`

- Retrieve all information for all rule sets.

### `/providers/rules/providers_name`

Request method: `PUT`

- Update the rule set.

## Connections

### `/connections`

Request method: `GET`

- Retrieve connection information.

Request method: `DELETE`

- Close all connections.

### `/connections/:id`

Request method: `DELETE`

- Close a specific connection.

## Domain Query

### `/dns/query`

Request method: `GET`

- Retrieve DNS query data for a specified name and type
- the URL must include `?name=example.com&type=A`, modified as needed.

## DEBUG

`/debug` requires the kernel to be started with the [log level](../config/general.md#log-level) set to `debug`.

### `/debug/gc`

Request method: `PUT`

- Perform active garbage collection.

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

Access `http://${controller-api}/debug/pprof/heap?raw=true` in a browser to download this file, and upload it to [issues](https://github.com/MetaCubeX/Clash.Meta/issues) to report any problems you encounter.