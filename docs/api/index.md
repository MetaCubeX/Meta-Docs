---
hide:
  - navigation
#   - toc
---
# API 说明

## API 密钥

请求时附带  `'Authorization: Bearer secret'`  请求头，`secret` 为配置文件设置的 api 密钥

下文的 `${controller-api}`格式为配置文件中设置的 `ip:port`

curl 示例 `curl -H 'Authorization: Bearer secret'  http://${controller-api}//version`

## 日志

### `/logs`

请求方法：`GET`

* 获取实时日志

## 流量信息

### `/traffic`

请求方法：`GET`

* 获取实时流量，单位 kbps

## 内存信息

### `/memory`

请求方法：`GET`

* 获取实时内存占用，单位 kb

## 版本信息

### `/version`

请求方法：`GET`

* 获取 clash 版本

## 缓存

### `/cache/fakeip/flush`

### 请求方法：`POST`

* 清除 fakeip 缓存

## 运行配置

### `/configs`

请求方法：`GET`

* 获取基本配置

请求方法：`PUT`

* 重新加载基本配置
* URL 需携带 `?force=true` 强制执行，必须发送数据
* curl 示例：`curl "${controller-api}/configs?force=true" -X PUT -d '{"path": "", "payload": ""}'`

请求方法：`PATCH`

* 更新基本配置，传入需要修改的配置即可，传入的数据需以 json 格式传入
* 示例：`curl ${controller-api}/configs -X PATCH -d '{"mixed-port": 7890}'`

### `/configs/geo`

### 请求方法：`POST`

* 更新 GEO 数据库
* 必须发送数据，因更新后会自动重载一次配置
* curl 示例：`curl "${controller-api}/configs" -X POST -d '{"path": "", "payload": ""}'`

### `/restart`

### 请求方法：`POST`

* 重启内核
* 必须发送数据
* curl 示例：`curl "${controller-api}/restart " -X POST -d '{"path": "", "payload": ""}'`

### `/upgrade`

### 请求方法：`POST`

* 更新内核
* 必须发送数据，因更新后会自动重载一次配置
* curl 示例：`curl "${controller-api}/upgrade" -X POST -d '{"path": "", "payload": ""}'`

## 代理

### `/proxies`

请求方法：`GET`

* 获取代理信息

### `/proxies/:name`

请求方法：`GET`

* 获取具体的代理信息

请求方法：`PUT`

* 选择特定的代理

### `/proxies/:name/delay`

请求方法：`GET`

* 获取具体代理的延迟测试信息

## 规则

### `/rules`

请求方法：`GET`

* 获取规则信息

## 连接

### `/connections`

请求方法：`GET`

* 获取连接信息

请求方法：`DELETE`

* 关闭所有连接

### `/connections/:id`

请求方法：`DELETE`

* 关闭特定连接

## 代理集合

### `/providers/proxies`

请求方法：`GET`

* 获取所有代理集合的所有代理信息

### `/providers/proxies/:name`

请求方法：`GET`

* 获取特定代理集合的代理信息

请求方法：`PUT`

* 更新的代理集合

### `/providers/proxies/:name/healthcheck`

请求方法：`GET`

* 触发特定代理集合的健康检查

## 规则集合

### `/providers/rules`

### `/providers/rules/:name`

## 域名查询

### `/dns/query`

请求方法：`GET`

* 获取指定名称和类型的 DNS 查询数据

参数

* `name`（必填）：要查询的域名。
* `type`（可选）：要查询的 DNS 记录类型（例如，A、MX、CNAME 等）

示例：`GET /dns/query?name=example.com&type=A`

## DEBUG

浏览器打开 `http://${controller-api}/debug/pprof` 可查看原始 DEBUG 信息，其中：

* allocs 表示每个函数调用的内存分配情况，包括在堆栈上和堆上分配的内存大小以及内存分配次数。这个报告主要是为了帮助我们找到代码中存在的内存泄漏、内存频繁申请等问题。
* heap 报告则给出了程序在堆上使用的内存的详细信息，其中包括被分配的内存块的大小、数量和地址，并且按照大小排序。这个报告主要是为了搜寻内存使用过高的地方，我们可以在 heap 报告中查看对象的大小，从而找到内存使用过高的地方。

### 安装 [Graphviz](https://graphviz.org/download/)，可查看图形化的 debug 信息：

#### 查看图形化 Heap 报告：

```
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/heap
```

[Full image](../assets/image/api/heap.svg)
<img src="../assets/image/api/heap.svg">

#### 查看图形化 Allocs 报告

````
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/allocs
````

[Full image](../assets/image/api/allocs.svg)
<img src="../assets/image/api/allocs.svg">

#### 提交输出报告

浏览器访问 `http://${controller-api}/debug/pprof/heap?raw=true` 即可下载这个文件，通过上传到 [issues](https://github.com/MetaCubeX/Clash.Meta/issues) 提交你遇到的问题。
