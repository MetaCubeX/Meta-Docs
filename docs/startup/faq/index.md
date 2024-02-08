---
hide:
  # - navigation
  - toc
---
### alpha 和 meta 分支的区别

alpha分支为最新提交分支，meta分支每隔一段时间合并alpha分支的代码，meta分支不一定比alpha分支更稳定。

### 我应该下载哪一个文件？

release 中，包的文件名中包含了多个信息，包括

* 程序名称（clash.meta）
* 操作系统（如android、darwin、freebsd、linux、windows等）
* 架构（如386、amd64、arm32v7、arm64等）
* 编译方式
>
> * `默认（无额外标识）`: 使用GOAMD64=v3标签编译的默认版本
> * `compatible`: 使用GOAMD64=v1标签进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
> * `go120`: 使用Golang1.20版本进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
>
* 分支（alpha）
* 提交的git hash值（如f90066f）

可以根据这些信息选择你需要下载的可执行文件。

👉[在此了解](https://github.com/golang/go/wiki/MinimumRequirements#amd64)更多关于 GOAMD64 标签的信息

👉[在此了解](https://go.dev/doc/go1.20#ports)更多关于Golang1.20版本的系统兼容性信息
