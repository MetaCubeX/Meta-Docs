# Meta-Docs

[mihomo](https://github.com/MetaCubeX/mihomo/tree/Alpha) 的文档

## 使用方法

### 设置虚拟环境

部分操作系统 python3.11 以上依赖虚拟环境运行，不能直接运行

##### 安装 virtualenv

- debian
  - bash/zsh/fish

  ```bash
  apt update && apt install virtualenv
  ```

  - nushell

  ```nushell
  apt update ; apt install virtualenv
  ```

- archlinux

  ```bash
  pacman -Syu python-virtualenv
  ```

##### 创建虚拟环境

### 安装 uv

```bash
推荐使用 [uv](https://docs.astral.sh/uv/) 管理 Python 和项目依赖。
```

Windows:

```powershell
winget install --id=astral-sh.uv
```

macOS / Linux:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 安装 Python

项目使用 Python 3.11，首次使用可执行：

```bash
uv python install
```

### 同步依赖

`uv` 会自动创建并管理项目根目录下的 `.venv`。

```bash
uv sync
```

### 设置镜像源（可选）

如果访问 PyPI 较慢，可以临时设置 `UV_DEFAULT_INDEX`：

- PowerShell

  ```powershell
  $env:UV_DEFAULT_INDEX = "https://pypi.tuna.tsinghua.edu.cn/simple"
  ```

- bash/zsh

  ```bash
  export UV_DEFAULT_INDEX="https://pypi.tuna.tsinghua.edu.cn/simple"
  ```

### 预览修改

```bash
uv run mkdocs serve
```

### 构建文档

```bash
uv run mkdocs build
```

### 使用虚拟环境（可选）

如果你确实需要手动进入虚拟环境，可以使用：

- PowerShell

  ```powershell
  .\.venv\Scripts\Activate.ps1
  ```

- bash/zsh

  ```bash
  source .venv/bin/activate
  ```

退出虚拟环境：

```bash
deactivate
```
