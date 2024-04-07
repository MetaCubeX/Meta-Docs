---
hide:
  - navigation
#   - toc
---
# API

## Request Example

curl example: `curl -H 'Authorization: Bearer ${secret}' http://${controller-api}/version`

This request includes the `'Authorization: Bearer ${secret}'` header, where:

- `${secret}`  is the [api](../config/general.md#api) secret set in the configuration file.
- `${controller-api}` is the [api](../config/general.md#api) listening address set in the configuration file.

## Logs

### `/logs`

Request Method: `GET`

- Obtain real-time logs.

## Traffic Information

### `/traffic`

Request Method: `GET`

- Obtain real-time traffic, measured in kbps.

## Memory Information

### `/memory`

Request Method: `GET`

- Obtain real-time memory usage, measured in kb.

## Version Information

### `/version`

Request Method: `GET`

- Get Clash version.

## Cache

### `/cache/fakeip/flush`

Request Method: `POST`

- Clear fake-IP cache

## Runtime Configuration

### `/configs`

Request Method: `GET`

- Get basic configuration.

Request Method: `PUT`

- Reload basic configuration.
- URL must include `?force=true` to force execution and must send data.
- curl example: `curl "${controller-api}/configs?force=true" -X PUT -d '{"path": "", "payload": ""}'`

Request Method: `PATCH`

- Update basic configuration by providing the configuration to be modified in JSON format.
- curl example: `curl ${controller-api}/configs -X PATCH -d '{"mixed-port": 7890}'`

### `/configs/geo`

Request Method: `POST`

- Update GEO database.
- Must send data, as it automatically reloads the configuration after the update.
- curl example: `curl "${controller-api}/configs" -X POST -d '{"path": "", "payload": ""}'`

### `/restart`

Request Method: `POST`

- Restart the kernel.
- Must send data.
- curl example: `curl "${controller-api}/restart " -X POST -d '{"path": "", "payload": ""}'`

## Update

### `/upgrade/`

Request Method: `POST`

- Update the kernel.
- Must send data, as it automatically reloads the configuration after the update.
- curl example: `curl "${controller-api}/upgrade" -X POST -d '{"path": "", "payload": ""}'`

### `/upgrade/ui`

Request Method: `POST`

- Update the panel; external-ui must be set.
- curl example: `curl "${controller-api}/upgrade/ui" -X POST`

## Proxies

### `/proxies`

Request Method: `GET`

- Get proxy information.

### `/proxies/:name`

Request Method: `GET`

- Get specific proxy information.

Request Method: `PUT`

- Select a specific proxy.

### `/proxies/:name/delay`

Request Method: `GET`

- Get delay test information for a specific proxy.

## Rules

### `/rules`

Request Method: `GET`

- Get rule information.

## Connections

### `/connections`

Request Method: `GET`

- Get connection information.

Request Method: `DELETE`

- Close all connections.

### `/connections/:id`

Request Method: `DELETE`

- Close a specific connection.

## Proxy Providers

### `/providers/proxies`

Request Method: `GET`

- Get information for all proxies in all Proxy Providers.

### `/providers/proxies/:name`

Request Method: `GET`

- Get information for proxies in a specific Proxy Providers.

Request Method: `PUT`

- Update Proxy Providers.

### `/providers/proxies/:name/healthcheck`

Request Method: `GET`

- Trigger health check for a specific  Proxy Providers.

## Rule Providers

### `/providers/rules`

Request Method: `GET`

- Get information for all Rule Providers.

### `/providers/rules/:name`

Request Method: `PUT`

- Update Rule Providers.

## Domain Query

### `/dns/query`

Request Method: `GET`

- Get DNS query data for a specified name and type.

Parameters:

- `name` (required): The domain name to query.
- `type` (optional): The DNS record type to query (e.g., A, MX, CNAME, etc.).

Example: `GET /dns/query?name=example.com&type=A`

## DEBUG

`/debug` requires the core to start with [debug log level](../config/general.md#_5)

### `/debug/gc`

Request Method: `PUT`

- Perform a manual GC.
- curl example: `curl "${controller-api}/debug/gc" -X PUT`

### `/debug/pprof`

Open `http://${controller-api}/debug/pprof` in a browser to view raw DEBUG information, including:

- `allocs` shows the memory allocation for each function call, including the size and number of allocations on the stack and heap. This report helps identify memory leaks and frequent memory allocations in the code.
- `heap` report provides detailed information about the program's use of memory on the heap, including the size, quantity, and address of allocated memory blocks, sorted by size. This report is useful for finding places where memory usage is too high. You can view object sizes in the heap report to identify areas of excessive memory usage.

#### Install [Graphviz](https://graphviz.org/download/) to view graphical debug information

##### View Graphical Heap Report

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/heap
```

[Full image](../assets/image/api/heap.svg)

##### View Graphical Allocs Report

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/allocs
```

[Example output](../assets/image/api/allocs.svg)

##### Submit Output Report

Access `http://${controller-api}/debug/pprof/heap?raw=true` in your browser to download the file and submit any issues you encounter by uploading it to [issues](https://github.com/MetaCubeX/Clash.Meta/issues).
