# agent-lab-notes

过去几年，科研领域最大的变化或许不是某个新模型的出现，而是 Agent 开始能够真正调用工具、读取文档、编写代码、执行任务，并参与完整的科研流程。

本仓库整理了我在实际科研工作中使用各类 Agent 工具的经验与笔记，包括环境配置、工具选型、工作流设计以及常见问题解决方案。目标不是介绍某一个产品，而是帮助科研同学建立属于自己的 AI 工具链，让更多时间用于思考问题本身，而不是消耗在重复性工作上。

> 分享的大部分内容不绑定特定智能体，可以在 Claude Code、Codex 等智能体上都有效。

## 1. 基础环境

- 📄[Git 指南](others/git.md)
- 📄[UV 指南](programme-env/uv.md)
- 📄[nodejs 指南](programme-env/nodejs.md)
- 📄[Miniforge 指南](programme-env/miniforge.md)

## 2. 系统与运行环境

- 📄[WSL 指南](operating-system/wsl.md)
- 📄[linux 之 Ubuntu 常用命令与配置](operating-system/linux.md)
- 📄[Hyper-V 指南](operating-system/Hyper-V.md)

## 3. 智能体

### 3.1 Claude Code

- 📄[Claude Code 指南](agents/claude-code/claude-code.md)
  - 📺[国内 Claude Code 安装保姆级教程](https://www.bilibili.com/video/BV1AjGD6mEV4)
- 🚀[Claude Code 一键快捷启动方式，下载放桌面](agents/claude-code/cc.bat)
- 📄[CLAUDE.md - 全局指令示范文件](agents/claude-code/CLAUDE.md)
- 📄[Claude Code 常用命令](agents/claude-code/tutorial/常用命令.md)
  - 📺[常用命令视频](https://www.bilibili.com/video/?p=1)
- 📄[Claude Code 交互模式](agents/claude-code/tutorial/交互模式.md)
  - 📺[交互模式视频](https://www.bilibili.com/video/?p=2)
- 📄[Claude Code 最佳实践](agents/claude-code/tutorial/最佳实践.md)
  - 📺[最佳实践视频](https://www.bilibili.com/video/?p=3)

### 3.2 Codex

- 📄[Codex 指南](agents/codex/codex.md)
  - 📄[Codex 指南 - 防吞备用 github 地址](https://github.com/psiQAQ/agent-lab-notes/blob/main/agents/codex/codex.md)
- 📄[AGENTS.md - 全局指令示范文件](agents/codex/AGENTS.md)
- 🧾[查询 ChatGPT 套餐周额度重置次数和时间限制脚本](agents/codex/codex-reset-remaining.py)

> 注意：使用 GPT 模型时，推荐使用 Codex，其他模型推荐使用 Claude Code。

## 4. 智能体扩展

### 4.1 Skills

- 📄[Skills](agents/skills/skills.md)
- 📄[ppt](agents/skills/pptx-related-skills.md)

### 4.2 MCP

- 📄[Context7 MCP](agents/MCP/context7.md)
- 📄[Exa](agents/MCP/Exa.md)
- 📄[gh_grep](agents/MCP/gh_grep.md)

### 4.3 周边工具与扩展

- 📄[claude-tap](agents/tools/claude-tap.md)
- 📄[ccstatueline](agents/tools/ccstatueline.md)
- 📄[blender 指南](others/blender.md)

## 5. 科研助力

- 📄[zotero 指南](others/zotero.md)
- 📄[academic-research-skills](agents/plugins/academic-research-skills.md)

## 6. 大模型选型与排行榜

- 📄[Models.dev｜AI 模型规格、价格与能力查询](models/models-dev.md)
- 📊[Artificial Analysis｜AI模型评测与API性能分析](https://artificialanalysis.ai/)
- ⚔️[Arena AI｜大模型竞技场与排行榜](https://arena.ai/)

## 7. AI 新闻

- 📰[24 小时 AI 更新雷达](https://learnprompt.github.io/ai-news-radar/)
- 📰[AIHOT](https://aihot.virxact.com/)

## 8. 补充资料

### bilibili-技术爬爬虾

- 📺[Codex (APP) 保姆级全攻略，海量实战教程， 一期精通Codex](https://www.bilibili.com/video/BV1Kk9kBAEJv)
- 📺[Git+Github核心概念大串讲，从零到一全攻略，详细实战教程](https://www.bilibili.com/video/BV1ySLc6QEcB)

### bilibili-张司机在路上

- 📺[解密Claude Code和Anthropic后端是如何通信](https://www.bilibili.com/video/BV1G2o5BqELx)
- 📺[教你最大化Claude Code缓存命中来节省token](https://www.bilibili.com/video/BV1ZQ5u6bEJ7)

### 其他

- 📺[【Markdown 完全指南】5 分钟上手，让你的笔记永远不被软件绑架！](https://www.bilibili.com/video/BV1tJXZBgEoC)
- 📺[Zotero8/9通用零基础教程](https://www.bilibili.com/video/BV1ecQBBZESv)
- 📺[codex提升科研效率100倍](https://www.bilibili.com/video/BV1NwEb6gEy1)
- 📺[万字拆解AI Agent编年史](https://www.bilibili.com/video/BV1NL9tBsELS)
- 📺[对姚顺宇的4小时访谈](https://www.bilibili.com/video/BV1YR5E6EE9o)
- 📺[中美大模型差距过去一年变大还是缩小？- Hugging Face | 王铁震](https://www.bilibili.com/video/BV1HDVT6bE8x)
- 📺[跟三个大厂码农聊了一晚上，程序员眼中AI叙事的真相原来是这样](https://www.bilibili.com/video/BV1gyEd6xEyu)
