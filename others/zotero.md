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
- Zotero MCP Plugin
- ...
- 按照 Star 数排序选择想要的插件进行安装
- ...

## MCP 调用 zotero 管理文献

安装 `Zotero MCP Plugin` 后，点击 `编辑` -> `设置` -> `Zotero MCP Plugin`

1. 启用写入操作
2. `客户端配置` 点击 `生成配置` -> `复制配置`
3. 默认是配置到项目中
    `claude mcp add --transport http zotero-mcp http://127.0.0.1:23120/mcp`
4. 也可以配置到用户目录下，所有项目可用
    `claude mcp add --scope user --transport http zotero-mcp http://127.0.0.1:23120/mcp`
5. 启动 `Claude Code`，输入 `/mcp`，检查是否连接成功
   - `zotero-mcp · √ connected · 24 tools`

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
