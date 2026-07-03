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

<https://gh-proxy.org//https://github.com/syt2/zotero-addons/releases/download/V9.0.2/zotero-addons.xpi>

打开 `zotero`，点击 `工具` -> `插件`，将下载的 `xpi` 文件拖入窗口中即可完成安装。

回到主界面，点击新增的 `拼图` 按钮，在弹出的窗口中搜索并安装其他插件。

## 插件推荐

- Translate for Zotero
- Better BibTeX for Zotero
- Zotero MCP Plugin（建议不装这个，后续介绍我开发的功能更多的派生版本）
- ...
- 按照 Star 数排序选择想要的插件进行安装
- ...

## 智能体 调用 zotero MCP 管理 zotero 的文献

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

### zotero-mcp 二次开发

本人以 `cookjohn/zotero-mcp` 为模板开发了 `zotero-mcp-dev`，并发布 `v1.6.0`，主要改进为：

* 新增 `run_javascript`（进程内 JS 调试/扩展执行）
* 新增文献工作流工具（缺失PDF检测、撤稿检查、关联分析、去重合并、批量标签等）
* 增强搜索（RRF融合 + 自适应加权混合检索）
* 改进导入与去重（DOI/ADS/Extra 结构优化）
* 增加写操作分级控制（dry-run + write.enabled）

> 详情见：[Release Tag](https://github.com/psiQAQ/zotero-mcp-dev/releases/tag/1.6.0)

仓库地址见
- [github 版](https://github.com/psiQAQ/zotero-mcp-dev)
- [gitcode 版](https://gitcode.com/RepoPorter/zotero-mcp-dev/blob/feat/psk-eval-run-js/README.md)

安装过程在 `README.md` 中有详细说明。

## 文献导入

提示词参考：

```plain
帮我检查pdf文件夹中的文献，将其全部导入到zotero中;
创建目录进行归类，可多级目录，分类依据为研究方向;
每篇文献从摘要中生成标签，用于筛选使用；
python 需要使用是优先使用项目的 uv 环境。
```

```plain
按照PRL的引用格式，利用 Better BibTeX for Zotero 插件帮我导出为 bib 文件
```
