# Meta-Docs

[Clash.Meta](https://github.com/MetaCubeX/Clash.Meta/tree/Alpha) 的文档

## 使用方法

### 设置虚拟环境

python3.11以上依赖虚拟环境运行,不能直接运行

##### 安装virtualenv

* debian
  * bash/zsh/fish
  ```bash
  apt update && apt install virtualenv
  ```
  * nushell
  ```nushell
  apt update ; apt install virtualenv
  ```

* archlinux
  ```bash
  pacman -Syu python-virtualenv
  ```

##### 创建虚拟环境

```bash
virtualenv venv
```

##### 使用虚拟环境
* bash/zsh
```bash
source venv/bin/activate
```
* nushell
```nushell
use venv/bin/activate.nu
```
* fish
```fish
source venv/bin/activate.fish
```

##### 退出虚拟环境

```bash
deactivate
```

##### 确认是否处于虚拟环境

```bash
which python3
```

### 设置源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 安装依赖

```bash
pip install -r requirements.txt
```

### 预览修改

```bash
mkdocs serve
```

### 参考文档

https://squidfunk.github.io/mkdocs-material/setup/
https://squidfunk.github.io/mkdocs-material/reference/

推荐使用 [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 对文档进行格式化。
