---
hide:
  # - navigation
  - toc
---
# FAQ

## Alpha and Meta Branch Differences

The Alpha branch is the latest commit branch, while the Meta branch merges the code from the Alpha branch at regular intervals. The Meta branch is not necessarily more stable than the Alpha branch.

## Which file should I download?

In`release`, the filename of each package includes several pieces of information, including:

* Program name (`mihomo`)
* Operating system (e.g., `android`, `darwin`, `freebsd`, `linux`, `windows`, etc.)
* Architecture (e.g., `386`, `amd64`, `arm32v7`, `arm64`, etc.)
* Compilation method:
>
> * `default(not specified in file name)`: Default version compiled with GOAMD64=v3 tag.
> * `compatible`: Compiled with GOAMD64=v1 tag for compatibility with specific OS or architecture.
> * `go120`: Compiled with Golang1.20 for compatibility with specific OS or architecture.
> * `abi1/2`: ABI version for `loongarch64`, specific details can be found at <http://www.loongnix.cn/zh/toolchain/Golang/downloads-Go1.21/index.html>
>
* Compile branch (e.g., `alpha`)
* Git hash value of the commit (e.g., `f90066f`)

You can choose the executable file you need based on these pieces of information.

Check details between different architectural levels [here](https://github.com/golang/go/wiki/MinimumRequirements#amd64).

Check details of system compatibility information about Golang version 1.20 [here](https://go.dev/doc/go1.20#ports).
