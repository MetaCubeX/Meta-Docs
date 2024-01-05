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
> * `默认（无额外标识）`: 使用GOAMD64=v3标签编译的默认版本。 
> * `compatible`: 使用GOAMD64=v1标签进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
> * `go120`: 使用Golang1.20版本进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
* 分支（alpha）
* 提交的git hash值（如f90066f）

可以根据这些信息选择你需要下载的可执行文件。

👉[在此了解](https://github.com/golang/go/wiki/MinimumRequirements#amd64)更多关于 GOAMD64 标签的信息

👉[在此了解](https://go.dev/doc/go1.20#ports)更多关于Golang1.20版本的系统兼容性信息

### Which file should I download?

In`release`, the filename of each package includes several pieces of information, including:

* Program name (`clash.meta`)
* Operating system (e.g., `android`, `darwin`, `freebsd`, `linux`, `windows`, etc.)
* Architecture (e.g., `386`, `amd64`, `arm32v7`, `arm64`, etc.)
* Compilation method:
> * `default(not specified in file name)`: Default version compiled with GOAMD64=v3 tag.
> * `compatible`: Compiled with GOAMD64=v1 tag for compatibility with specific OS or architecture.
> * `go120`: Compiled with Golang1.20 for compatibility with specific OS or architecture.
* Compile branch (e.g., `alpha`)
* Git hash value of the commit (e.g., `f90066f`)

You can choose the executable file you need based on these pieces of information.


Check details between different architectural levels [here](https://github.com/golang/go/wiki/MinimumRequirements#amd64).

Check details of system compatibility information about Golang version 1.20 [here](https://go.dev/doc/go1.20#ports).
