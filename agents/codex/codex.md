# Codex 指南

Codex CLI 是 OpenAI 的本地终端代码智能体，会在当前目录读取、修改、运行代码；ChatGPT Plus、Pro、Business、Edu、Enterprise 计划包含 Codex。首次运行 `codex` 会提示登录 ChatGPT 账号或使用 API key。

## 安装建议

### 桌面端（APP）版本安装

Windows 用户推荐优先通过 Microsoft Store 安装 Codex 桌面版。

### 命令行（CLI）版本安装

Codex CLI 安装命令合集（Windows / macOS / Linux / WSL）

```bash
# Windows
# PowerShell 官方脚本：最官方但依赖外网；通常需要代理
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
# winget：Windows 原生最稳方案；一般不需要代理但受网络影响
winget install -e --id OpenAI.Codex
# npm：跨平台开发方式；需要 Node 且通常需要代理
npm install -g @openai/codex

# macOS
# brew：最稳定 mac 安装方式；可能需要代理访问 GitHub
brew install --cask codex
# 官方脚本：最新版本但依赖外网；通常需要代理
curl -fsSL https://chatgpt.com/codex/install.sh | sh

# Linux / WSL
# 官方脚本：最简单方式；依赖 GitHub/OpenAI，通常需要代理
curl -fsSL https://chatgpt.com/codex/install.sh | sh
# npm：开发者常用方式；需 Node 环境且通常需要代理
npm install -g @openai/codex

# 验证（全平台）
codex --version && codex
```

## 代理配置

通过配置环境变量的方式，让 Codex 访问 GPT 模型时使用本地代理。

分为两类：

- `system env (setx/export)`：系统级环境变量，对所有应用生效；
- `.env` 文件：Codex CLI 读取的环境变量文件，优先级低于系统环境变量。

### 方式一：.env 文件（推荐）

下面给你三套系统（Windows / macOS / Linux/WSL）统一写入 `~/.codex/.env` 的标准方式，保证行为一致。

```bash
# Windows（PowerShell，推荐）
# 创建目录
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex"
# 写入 .env（覆盖），或者直接复制 4 行内容到文件中
@"
HTTP_PROXY=http://127.0.0.1:7890
HTTPS_PROXY=http://127.0.0.1:7890
ALL_PROXY=http://127.0.0.1:7890
NO_PROXY=localhost,127.0.0.1,192.168.0.0/16,10.0.0.0/8,::1
"@ | Set-Content -Encoding UTF8 "$env:USERPROFILE\.codex\.env"

# macOS / Linux / WSL
# 创建目录
mkdir -p ~/.codex
# 写入 .env（覆盖），或者直接复制 4 行内容到文件中
cat > ~/.codex/.env << 'EOF'
HTTP_PROXY=http://127.0.0.1:7890
HTTPS_PROXY=http://127.0.0.1:7890
ALL_PROXY=http://127.0.0.1:7890
NO_PROXY=localhost,127.0.0.1,192.168.0.0/16,10.0.0.0/8,::1
EOF

# 验证（所有系统通用）
cat ~/.codex/.env
```

### 方式二：环境变量

设置用户级环境变量。

假设本地代理端口是 `7890`：

```bash
setx HTTP_PROXY http://127.0.0.1:7890
setx HTTPS_PROXY http://127.0.0.1:7890
setx ALL_PROXY http://127.0.0.1:7890
setx NO_PROXY "localhost,127.0.0.1,192.168.0.0/16,10.0.0.0/8,::1"
```

设置完成后，需要重新打开终端或重启 Codex，使环境变量生效。

如果使用 PowerShell，也可以检查当前环境变量：

```powershell
echo $env:HTTP_PROXY
echo $env:HTTPS_PROXY
echo $env:ALL_PROXY
echo $env:NO_PROXY
```

如果要取消代理变量，可以在系统环境变量设置中删除对应项目，或使用注册表/PowerShell 清理用户环境变量：

```bash
# Windows（PowerShell）
# 删除用户环境变量
reg delete HKCU\Environment /v HTTP_PROXY /f
reg delete HKCU\Environment /v HTTPS_PROXY /f
reg delete HKCU\Environment /v ALL_PROXY /f
reg delete HKCU\Environment /v NO_PROXY /f
```

## 全局指令

`AGENTS.md` 是 Codex 读取的智能体指令文件，已根据 [OpenAI GPT-5.6 Sol 提示指南](https://developers.openai.com/api/docs/guides/prompt-guidance-gpt-5p6) 适配，采用精简、结果导向的写法。当前指令覆盖：

- 沟通规范（中文为主）与输出格式；
- 任务执行框架：目标、成功标准、约束、验证、停止条件；
- 编码原则：先思考后编码、简洁优先、精准修改、目标驱动；
- 自主权与审批边界：区分可直接执行与需确认的操作；
- Python 环境与依赖管理；
- Git 工作流与安全边界；
- 工具选择与子 Agent 协作规范；
- 文档编写与验证要求；
- Windows `.bat` 脚本规则。

通常有两类用法：

| 类型 | 推荐位置 | 作用 |
| ------- | ---------------------------------------- | ----------- |
| 用户级全局指令 | `%USERPROFILE%\.codex\AGENTS.md` | 对所有项目生效 |
| 项目级指令 | 项目根目录下的 `AGENTS.md` | 只对当前项目生效 |
| 子目录局部指令 | 子目录下的 `AGENTS.md` 或 `AGENTS.override.md` | 对特定模块或子项目生效 |

### 推荐指令文件参考

[AGENTS.md](./AGENTS.md) — 已按 [GPT-5.6 Sol 提示指南](https://developers.openai.com/api/docs/guides/prompt-guidance-gpt-5p6) 精简优化，去掉重复规则和过度指令，仅保留结果导向的关键约束。

先下载到本地并移动到 `%USERPROFILE%\.codex\AGENTS.md` or `~/.codex/AGENTS.md`，对所有项目生效，或使用以下参考命令：

```bash
# Windows（Powershell）
mkdir "%USERPROFILE%\.codex"
Move-Item AGENTS.md "%USERPROFILE%\.codex\AGENTS.md"

# macOS / Linux / WSL
mkdir ~/.codex
mv AGENTS.md ~/.codex/AGENTS.md
```

也可以将 `AGENTS.md` 放在项目根目录中，让 Codex 针对当前项目加载专门规则。

> 补充说明：OpenAI 帮助文档也提到可以在 Codex 应用中用 `/init` 为当前项目生成 `AGENTS.md` 脚手架。[3]

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

### 示例配置

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

- Codex 桌面版是否为最新版本；
- ChatGPT 手机端是否为最新版本；
- 两端是否登录同一个账号；
- 本地网络或代理是否拦截连接；
- `config.toml` 中是否启用了 `remote_connections`。

参考📺[2026 最新｜Codex 手机远程连接 + 代理配置保姆级教程][2]

## 查询周额度重置次数

查询 ChatGPT 套餐周额度重置次数和时间限制，通常需要代理访问 ChatGPT Web 后端接口，这里提供一个脚本供你参考：

[codex-reset-remaining.py](./codex-reset-remaining.py)

使用 python 执行脚本或者让 Codex 帮你执行并返回结果，结果格式如下：

```text
Codex 重置次数（北京时间）
可用次数：4

分类：Codex 速率限制重置（codex_rate_limits，4 个）
1. 状态：可用
   创建时间：2026-06-12 11:16:50 北京时间
   到期时间：2026-07-12 11:16:50 北京时间
2. 状态：可用
   创建时间：2026-06-18 08:07:44 北京时间
   到期时间：2026-07-18 08:07:44 北京时间
3. 状态：可用
   创建时间：2026-06-27 07:32:03 北京时间
   到期时间：2026-07-27 07:32:03 北京时间
4. 状态：可用
   创建时间：2026-07-02 03:48:51 北京时间
   到期时间：2026-08-01 03:48:51 北京时间
```

> 脚本原理：读取本地 Codex 登录态里的 ChatGPT access token，然后调用 ChatGPT 后端的“rate-limit reset credits”接口，最后把返回的重置次数和到期时间格式化输出为北京时间。它不是通过 Codex CLI 官方命令查询，而是直接访问 ChatGPT Web 后端接口。

[1]: https://developers.openai.com/codex/config-basic "Config basics – Codex | OpenAI Developers"
[2]: https://developers.openai.com/codex/guides/agents-md "Custom instructions with AGENTS.md – Codex | OpenAI Developers"
[3]: https://help.openai.com/zh-hans-cn/articles/11369540-using-codex-with-your-chatgpt-plan "在你的 ChatGPT 套餐中使用 Codex | OpenAI Help Center"
