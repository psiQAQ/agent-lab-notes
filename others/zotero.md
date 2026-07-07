# zotero 指南

Zotero 是文献管理工具，可用于收集、整理、标注和引用论文、网页与书籍资料。这篇文档从本体安装开始，随后讲 Edge 插件、中文社区、插件扩展、通过 MCP 管理文献，以及文献导入流程。

## 下载安装

<https://www.zotero.org/download/>

安装时选择默认选项即可。

## Edge 插件

推荐使用 Edge 插件，点击 `Install` -> `获取` -> `添加扩展`。

## Zotero 中文社区

<https://zotero-chinese.github.io/>

## 插件安装

Zotero 中文社区的插件地址：

<https://zotero-chinese.github.io/plugins/>

第一步安装 `Add-on Market for Zotero`，方便后续安装其他插件。

[插件市场 xpi 下载镜像地址](https://gh-proxy.org//https://github.com/syt2/zotero-addons/releases/download/V9.0.2/zotero-addons.xpi)

打开 `zotero`，点击 `工具` -> `插件`，将下载的 `xpi` 文件拖入窗口中即可完成安装。

回到主界面，点击新增的 `拼图` 按钮，在弹出的窗口中搜索并安装其他插件。

## 插件推荐

### 常用插件推荐

- Translate for Zotero
- Better BibTeX for Zotero
- Zotero MCP Plugin（建议不装这个，后续介绍我开发的功能更多的派生版本）
- ...
- 选择想要的插件进行一键安装，插件均支持自动更新。
- ...

### Zotero 与 AI 智能体联动

#### AI 控制 zotero 现有方案

目前调用 AI 管理文献有多个实现的方案，以下是一些案例的比较：

| | **54yyyu/zotero-mcp** | papersgpt-for-zotero | mcp-server-zotero-dev | ZotSeek | **cookjohn/zotero-mcp** |
|---|---|---|---|---|---|
| 形态 | 纯 Python MCP server（外部） | AI 读论文插件 | Node MCP + RDP 桥插件 | 语义搜索插件 + 只读 MCP | **插件内嵌完整 MCP server** |
| 与 Zotero | pyzotero → 23119（官方 API） | 插件内 + 外部闭源 sidecar | 插件开 RDP 端口，Node 端连 | 挂官方 23119 endpoint | 插件自开 23120 |
| ① 独立端口 | ✗（复用 23119，且是消费方） | ✗（9080 在闭源外部进程） | ✅ 6100（Firefox RDP） | ✗（挂 23119） | ✅ **23120**（nsIServerSocket） |
| ② 工具集 | ✅✅ **最全（约 61 个）** 读/搜/注释/导入/语义/scite | ✗ 0 | ✗ 0（28 个全调试类） | ✗ 3 个只读 | ✅ **27 个**（18 读 + 9 写） |
| ③ JS eval | ✗（外部，进不去进程） | 仅可行性证明（`window.eval` 无沙箱） | ✅ 完整实现（项目核心） | ✗（有 `/open` 原型） | ✗（插入点明确，约 50 行补上） |
| ④ PSK 认证 | ✗（stdio；用 web API key，本地写场景无意义） | ✗ | ✗ **RDP 协议层无法加装** | ✗（仅 Origin 校验） | ✗（单点插入 Bearer，约 20 行补上） |
| **写操作** | ✗ **local 模式 501，且吞错误返回乐观成功** | eval 内 `saveTx` | eval 内任意 | 刻意只读 | **直接 DataObject API，默认门禁关** |
| Zotero 9.0.4 兼容 | ✅（HTTP 客户端，不吃版本） | ❌ manifest 锁 7.0.* | ✅ max 10.* | ✅ 8–9.* | ✅ 6.999–10.99 |
| 活跃/许可 | 活跃 / — | ❌ 废弃快照(2025-01) | ✅ / MIT | ✅ / — | ✅2026-06 / **MIT** |

✅=具备可直接用｜✗=缺失｜数字=具体程度。

#### Zotero-Agent

为了让文献在智能体控制下实现更多功能，例如：

- 本地 PDF 批量导入
- PDF 元数据条目建立
- tag 标签建立
- 文献分类整理
- 搜索并下载相关文献
- .bib 引用生成
- 生成文献工作流总结，为后续操作提供参考

本人以 `cookjohn/zotero-mcp` 为模板开发了 [Zotero-Agent](https://github.com/psiQAQ/zotero-agent)，并发布 `v2.x.x`，主要改进为：

* 新增 `run_javascript`（进程内 JS 调试/扩展执行）
* 新增文献工作流工具（缺失PDF检测、撤稿检查、关联分析、去重合并、批量标签等）
* 增强搜索（RRF融合 + 自适应加权混合检索）
* 改进导入与去重（DOI/ADS/Extra 结构优化）
* 增加写操作分级控制（dry-run + write.enabled）

#### 安装过程

1. 安装 `zotero-agent` 插件

- （待插件市场更新，暂时推荐使用下面两种方式）前文安装的 `Add-on Market for Zotero` 插件中搜索 `zotero-agent`，右键点击 `安装` 即可
- [Release xpi 插件下载地址](https://github.com/psiQAQ/zotero-agent/releases)
- [xpi 下载镜像地址](https://gh-proxy.org//https://github.com/psiQAQ/zotero-agent/releases/download/v2.0.2/zotero-agent-2.0.2.xpi)

打开 `zotero`，点击 `工具` -> `插件`，将下载的 `xpi` 文件拖入窗口中即可完成安装。

2. 配置智能体客户端 `MCP` 服务

   1. 方式一（推荐）：在 `zotero` -> `编辑` -> `设置` -> `Zotero Agent` -> `客户端配置`，选择正在使用的客户端，复制配置，自行配置或交给智能体代为配置。

   2. 方式二：智能体阅读配置文档指导你配置：
      - [github 版](https://github.com/psiQAQ/zotero-agent/)
      - [国内可访问 gitcode 版](https://gitcode.com/RepoPorter/zotero-agent)


#### 提示词范例

* **本地 PDF 批量导入 + 元数据条目建立**
  请将本地文件夹 **xxx** 中的 PDF **批量导入 Zotero**。先扫描所有 PDF，识别 **重复文件**、**已有条目** 和 **新文件**；对新 PDF 提取 **DOI**、**arXiv ID**、**ISBN**、**PMID**、**标题** 等信息，并优先通过标识符建立标准 Zotero 文献条目。将 PDF 附加到对应条目下，补全 **作者**、**年份**、**期刊/会议**、**摘要**、**DOI** 等元数据，并加入 collection **xxx**。所有写入操作先输出 **dry-run 清单**，确认后再执行。

* **文献分类整理 + tag 标签建立**
  请整理 Zotero 中 collection **xxx** 的文献结构。根据 **标题**、**摘要**、**全文**、**已有 tag** 和 **注释**，将文献分类到子 collection：**xxx**、**xxx**、**xxx**、**xxx**、**xxx**。每篇文献可以进入多个子 collection。请同时建立统一 **tag 体系**，合并 **大小写**、**连字符**、**下划线** 和 **同义重复 tag**。先输出 **分类表**、**tag 映射表** 和 **判断依据**，确认后再批量写入。

* **搜索并下载相关文献**
  请围绕研究主题 **xxx** 搜索并扩展相关文献。先检索 **Zotero 本地库**，再基于核心文献的 **引用**、**被引** 和 **相似论文** 继续扩展。对库中不存在但相关性高的文献，尝试通过 **DOI**、**arXiv ID**、**ISBN** 或 **PMID** 导入；如果能找到 **开放获取 PDF**，请下载并附加到 Zotero 条目。最后输出 **已存在文献**、**新增文献**、**无法导入文献** 和 **未能下载 PDF** 的清单。

* **文献注释综合 + 综述笔记生成**
  请读取 collection **xxx** 中所有文献的 **PDF 高亮**、**批注**、**笔记** 和 **摘要**，生成一份结构化文献综述笔记。请按 **研究背景**、**核心问题**、**方法路线**、**数据集**、**实验设置**、**评价指标**、**主要结论**、**局限性** 和 **可引用观点** 分类整理。每条结论都标注对应 Zotero 条目。先展示 **综述草稿**，确认后再写入 Zotero note。

* **项目收尾检查 + .bib 引用生成 + 工作流总结**
  请对 Zotero 项目 **xxx** 进行一次完整的文献工作流收尾检查。检查 **重复条目**、**缺失 PDF**、**缺失 DOI**、**缺失摘要**、**元数据不完整**、**tag 混乱**、**未分类文献** 和 **可能的撤稿风险**；修复前先输出 **dry-run 报告**。随后基于清理后的 collection 生成 **BibTeX**，citation key 格式为 **FirstAuthorYearShortTitle**，并生成一份 **工作流总结**，记录 **已完成操作**、**遗留问题** 和 **后续建议**。

