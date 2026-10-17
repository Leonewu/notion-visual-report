# Notion Visual Report

让 AI 写进 Notion 的报告、纪要和学习笔记，使用原生视觉块表达内容。

目标：**30 秒找到结论，5 分钟读懂细节。** 根据内容选择 callout、表格、Mermaid、分栏、tabs 和折叠块，避免整页堆砌 Markdown 正文。

[公开分享页](https://notion-visual-report-skill.freehorse4.chatgpt.site/) · [完整规则](SKILL.md) · [原生块样例](examples/report.notion.md) · [MIT License](LICENSE)

## 安装

需要支持自定义 Agent Skills 的客户端，以及已连接、具备目标页面写入权限的 Notion MCP。打开分享页本身不会安装 skill，也不会授予 Notion 权限。

### 交给支持安装 Skill 的客户端

复制以下提示：

```text
请从 https://github.com/Leonewu/notion-visual-report 安装 notion-visual-report 到我的个人 Skills。
保留 agents/openai.yaml 中的 policy.allow_implicit_invocation: true。
在我创建、改写或整理 Notion 文档时使用这个 skill。
```

### Codex 本地安装

下载仓库 ZIP，解压后把整个 skill 目录放到 `${CODEX_HOME:-$HOME/.codex}/skills/notion-visual-report`。目录中必须直接包含 `SKILL.md` 和 `agents/`。重新打开客户端或新建会话后，确认它出现在可用技能列表中。

也可以在没有同名目录时运行：

```bash
git clone https://github.com/Leonewu/notion-visual-report.git "${CODEX_HOME:-$HOME/.codex}/skills/notion-visual-report"
```

自动匹配需要客户端支持、安装启用和可用的 Notion 连接；配置允许自动选择，并不保证每个客户端都会在每次任务中选择它。

## 使用

安装后，直接描述你要完成的 Notion 写作任务：

```text
把这些会议记录整理到 Notion：先写决策，再列行动项，把背景讨论折叠起来。
```

```text
在 Notion 写一篇 Git rebase 与 merge 的选型分析，用对比表和流程图解释。
```

也可以显式调用：

```text
使用 $notion-visual-report，把下面的资料整理成 Notion 学习笔记。
```

指定目的页面或数据库时，skill 先读取目标再创建；没有指定时创建私有草稿。读取页面、改任务状态或只改数据库字段，不属于文档排版任务。

## 内容如何选择原生块

| 内容 | 展示方式 |
| --- | --- |
| 开篇结论 | 蓝色 TL;DR callout |
| 多维比较 | 原生 table |
| 流程与架构 | Mermaid |
| 两个并列观点 | columns |
| 同一主题的不同视角 | tabs |
| 可选深读内容 | toggle |
| 待办事项 | to-do |

内置选型分析、调研报告、摘要和学习笔记四类骨架。按内容取舍，不填充空章节。

## 验证与限制

- 每个会话先读取 Notion MCP 提供的当前 enhanced Markdown spec。
- 写入后 fetch 回读，核对保存后的块类型和嵌套关系。
- 视觉验收需打开实际页面，检查 Mermaid、表格、分栏和交互；仅写入或回读成功，不代表渲染通过。
- Mermaid 类型和视觉行为以目标 Notion 客户端实际表现为准。
- `examples/report.notion.md` 使用 Notion enhanced Markdown。GitHub 源码预览不能代替 Notion 页面渲染。

## 参与改进

欢迎提交 Issue 或 PR。报告问题时请附上客户端、复现步骤、预期与实际结果，并去除私人文档内容和凭据。

本项目独立维护，与 Notion 官方无隶属关系。
