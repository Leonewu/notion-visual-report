# Notion Visual Report

简体中文 · [English](README.en.md)

让 AI 写进 Notion 的报告、纪要和学习笔记，使用原生视觉块表达内容。

**30 秒找到结论，5 分钟读懂细节。** 支持选型分析、调研报告、摘要和学习笔记，用 callout、表格、Mermaid、分栏和折叠块组织内容。

[完整规则](SKILL.md) · [原生块样例](examples/report.notion.md) · [MIT License](LICENSE)

![Notion 实际效果：Git merge 与 rebase 选型报告，包含结论提示框、对比表、流程图和双栏建议](assets/notion-report.png)

*真实 Notion 页面截图；Mermaid 切换为仅预览，命令示例按需折叠。*

## 安装

需要支持 Agent Skills 的客户端，以及具备目标页面写入权限的 Notion MCP。将以下提示交给支持安装 skill 的客户端：

```text
请从 https://github.com/Leonewu/notion-visual-report 安装 notion-visual-report 到我的个人 Skills。
在我创建、改写或整理 Notion 文档时使用这个 skill。
```

### Codex 本地安装

没有同名目录时运行：

```bash
git clone https://github.com/Leonewu/notion-visual-report.git "${CODEX_HOME:-$HOME/.codex}/skills/notion-visual-report"
```

重新打开客户端或新建会话，确认 skill 已加载。自动匹配取决于客户端支持。

## 使用

```text
使用 notion-visual-report，在 Notion 写一篇 Git rebase 与 merge 的选型分析，
用对比表和流程图解释，保存到这页下面：<目标页面链接>。
```

也可以直接要求整理会议记录、资料摘要或学习笔记，并指定目标页面或数据库。

## 语言

优先使用你指定的输出语言；未指定时，修改页面保留原语言，新建页面跟随提问语言。只有无法判断时才默认中文。标题、表头、图表标签等随输出语言翻译，技术术语保留英文，代码与标识符保持原样。

例如，用中文提问「请用英文写这份报告」，仍会输出英文。上方截图是中文示例，不代表只支持中文。

## 验证与限制

- 写入前读取 Notion 当前 enhanced Markdown spec，写入后回读检查结构。
- 图表与交互需在实际 Notion 页面中检查；回读成功不代表视觉验收通过。
- 样例使用 Notion enhanced Markdown，不能用 GitHub 预览判断最终效果。

欢迎提交 Issue 或 PR，请去除私人文档内容和凭据。本项目与 Notion 官方无隶属关系。
