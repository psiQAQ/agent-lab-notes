# Codex 指南

Codex 是面向代码任务的 Agent / CLI 工具，可用于阅读项目代码、修改文件、执行命令、排查报错，并协助完成开发、测试、重构和文档维护等工作流。

## 安装建议

Windows 用户推荐优先通过 Microsoft Store 安装 Codex 桌面版。

对于代码修改、项目调试、命令行任务、批量文件处理和开发工作流，推荐优先使用 Codex，而不是普通聊天窗口。Codex 更适合在真实项目目录中执行任务，并能结合项目文件、终端命令和智能体指令完成连续开发操作。

## 全局指令

`AGENTS.md` 是 Codex 读取的智能体指令文件，用于告诉编码智能体：

* 项目如何安装、构建和测试；
* 修改代码时需要遵守哪些规范；
* 哪些目录、文件或命令需要特别注意；
* 什么时候需要运行测试、格式化或检查命令；
* 需要避免哪些危险操作。

通常有两类用法：

| 类型 | 推荐位置 | 作用 |
| ------- | ---------------------------------------- | ----------- |
| 用户级全局指令 | `%USERPROFILE%\.codex\AGENTS.md` | 对所有项目生效 |
| 项目级指令 | 项目根目录下的 `AGENTS.md` | 只对当前项目生效 |
| 子目录局部指令 | 子目录下的 `AGENTS.md` 或 `AGENTS.override.md` | 对特定模块或子项目生效 |

本人使用的指令文件参考：

[AGENTS.md](./AGENTS.md)

使用方式：

```bash
# Windows
mkdir "%USERPROFILE%\.codex"
copy AGENTS.md "%USERPROFILE%\.codex\AGENTS.md"
```

也可以将 `AGENTS.md` 放在项目根目录中，让 Codex 针对当前项目加载专门规则。

## 配置文件

在 Windows 中，Codex 的用户级配置文件位于（参考 [Config basics][1]）：

```text
%USERPROFILE%\.codex\config.toml
```

在 macOS / Linux 中，对应路径为：

```text
~/.codex/config.toml
```

项目级配置可以放在项目目录下：

```text
.codex/config.toml
```

用户级配置适合放通用偏好，例如默认模型、代理、功能开关、MCP 配置等；项目级配置适合放当前仓库专用设置。

示例配置：

```toml
[features]
js_repl = false
memories = true
remote_connections = true
network_proxy = true
```

其中：

| 配置项 | 作用 |
| -------------------- | -------------------- |
| `js_repl` | 是否启用 JavaScript REPL |
| `memories` | 是否启用记忆相关能力 |
| `remote_connections` | 是否启用远程连接能力 |
| `network_proxy` | 是否启用网络代理相关能力 |

## 手机远程连接

Windows / macOS 下的 Codex 桌面版可配合手机端 ChatGPT App 使用远程连接能力。

一般流程是：

1. 在 Codex 桌面版中开启远程连接相关功能；
2. 确保 ChatGPT 手机端登录同一个账号；
3. 按桌面端提示完成连接；
4. 在手机端查看或控制 Codex 会话。

如果连接失败，优先检查：

* Codex 桌面版是否为最新版本；
* ChatGPT 手机端是否为最新版本；
* 两端是否登录同一个账号；
* 本地网络或代理是否拦截连接；
* `config.toml` 中是否启用了 `remote_connections`。

## 代理配置

如果网络环境需要代理，可以在 Windows 的 `cmd` 中设置用户级环境变量。

假设本地代理端口是 `7890`：

```bash
setx HTTP_PROXY http://127.0.0.1:7890
setx HTTPS_PROXY http://127.0.0.1:7890
setx ALL_PROXY http://127.0.0.1:7890
setx NO_PROXY "localhost,127.0.0.1,::1"
```

设置完成后，需要重新打开终端或重启 Codex，使环境变量生效。

如果使用 PowerShell，也可以检查当前环境变量：

```powershell
echo $env:HTTP_PROXY
echo $env:HTTPS_PROXY
echo $env:ALL_PROXY
echo $env:NO_PROXY
```

如果要取消代理变量，可以在系统环境变量设置中删除对应项目，或使用注册表/PowerShell 清理用户环境变量。

## 推荐使用场景

Codex 更适合处理以下任务：

| 场景 | 示例 |
| ------ | ------------------------------------ |
| 项目阅读 | 总结代码结构、分析模块依赖、解释启动流程 |
| 报错修复 | 根据 traceback / log 定位问题并修改代码 |
| 批量修改 | 批量重命名、格式调整、替换配置、迁移接口 |
| 自动测试 | 运行测试、根据失败结果继续修复 |
| 文档维护 | 生成 README、补充安装说明、整理使用教程 |
| 工程配置 | 修改 `config.toml`、`.gitignore`、CI、脚本等 |
| 命令行工作流 | 执行构建、安装依赖、检查环境、生成文件 |

## 参考资料

* [AGENTS.md](./AGENTS.md)
* [2026 最新｜Codex 手机远程连接 + 代理配置保姆级教程][2]

补充说明：OpenAI 帮助文档也提到可以在 Codex 应用中用 `/init` 为当前项目生成 `AGENTS.md` 脚手架。[3]

[1]: https://developers.openai.com/codex/config-basic "Config basics – Codex | OpenAI Developers"
[2]: https://developers.openai.com/codex/guides/agents-md "Custom instructions with AGENTS.md – Codex | OpenAI Developers"
[3]: https://help.openai.com/zh-hans-cn/articles/11369540-using-codex-with-your-chatgpt-plan "在你的 ChatGPT 套餐中使用 Codex | OpenAI Help Center"
