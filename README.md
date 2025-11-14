forked from [github/spec-kit](https://github.com/github/spec-kit)

## 项目介绍

这是一个服务于 cozeloop 开源仓库的基于 spec-kit 工具。


## ⚡ 快速开始

### 1. 安装 Specify CLI

选择您偏好的安装方式：

#### 选项 1：持久化安装（推荐）

一次安装，随处使用：

```bash
uv tool install specify-cli --from git+https://github.com/ykg3211/x-spec-kit.git
```

然后直接使用该工具：

```bash
specify init <项目名称>
specify check
```

要升级 specify，请运行：

```bash
uv tool install specify-cli --force --from git+https://github.com/ykg3211/x-spec-kit.git
```

#### 选项 2：一次性使用

无需安装，直接运行：

```bash
uvx --from git+https://github.com/ykg3211/x-spec-kit.git specify init <项目名称>
```

**持久化安装的优势：**

- 工具保持安装状态并存在于 PATH 中
- 无需创建 shell 别名
- 通过 `uv tool list`、`uv tool upgrade`、`uv tool uninstall` 更好地管理工具
- 更简洁的 shell 配置

## 初始化项目

要初始化一个新的 Specify 项目，请运行：

```bash
specify init <项目名称>
```

如果需要使用基础模板，添加 `--base` 参数：
带 `--base` 参数会从 [github/spec-kit](https://github.com/github/spec-kit) 初始化项目
如果不带则在本仓库初始化项目

```bash
specify init <项目名称> --base
```
