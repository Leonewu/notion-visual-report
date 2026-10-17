---
name: notion-visual-report
description: Use when creating, writing, updating, restructuring or formatting Notion documents, including reports, summaries, analyses, meeting notes, study notes, plans and technical documentation. Choose native Notion visual blocks for readable pages. Applies to document writing even when the user does not name the skill; excludes read-only lookups and property-only or database-schema changes.
---

# Notion Visual Report

Goal: a Notion page that can be skimmed in 30 seconds and read in depth in 5 minutes. Plain walls of Markdown text are a failure.

## Automatic invocation

- Apply to Notion document creation and substantive content edits, including requests such as “写到 Notion”, “整理成 Notion 笔记” and “update my Notion report”.
- Preserve `policy.allow_implicit_invocation: true` in `agents/openai.yaml`. The client must support skills, have this skill installed and enabled, and have a working Notion connection. Automatic selection depends on the client.
- For existing documents, apply the visual rules to the requested scope while preserving unrelated content and native references.

## Before writing

1. Read the Notion enhanced Markdown spec (`notion://docs/enhanced-markdown-spec`, via the Notion MCP fetch tool) once per session. Use the current spec for exact syntax. If it cannot be fetched, report the limitation rather than inventing block syntax.
2. Decide the page skeleton from the content type (see Templates below).
3. Use the user's explicitly requested output language first. Otherwise, preserve the existing page's language when editing, or use the user's request language for a new page. Fall back to Chinese only when none of these determines a language. Keep technical terms in English and code, commands, API names and identifiers unchanged.

## Required elements on every report page

- Page icon: one emoji matching the topic. Set the title in properties, without repeating it in content.
- A TL;DR callout at the top: `<callout icon="💡" color="blue_bg">` with the conclusion in 1–3 sentences and the key phrase in bold.
- `<table_of_contents/>` if the page has 4+ sections.
- At least one content-matched visual block: table, Mermaid, columns, tabs or a toggle containing useful detail. Choose from the mapping below; avoid adding an unrelated diagram just to satisfy this requirement.
- A closing summary: one-line quote block or a decision table.

## Content → block mapping

| Content shape | Use |
| --- | --- |
| Comparing 2–4 options on several dimensions | `<table header-row="true" header-column="true">` |
| Process, flow, architecture, state machine, git history, timeline | Fenced `mermaid` block: flowchart, sequenceDiagram, gitGraph, timeline or stateDiagram |
| Two options side by side, pros vs cons | `<columns>` with two `<column>` children |
| Warnings, pitfalls, key rules | `<callout icon="⚠️" color="red_bg">` or `yellow_bg` |
| Tips | `<callout icon="✅" color="green_bg">` |
| Deep detail most readers can skip, raw data, long examples | Toggle `<details><summary>` or toggle heading `## … {toggle="true"}` |
| Alternative views of the same thing, such as per platform or role | `<tabs>` with `<tab>` children |
| Commands, code, config | Fenced code block with language |
| Action items | To-do list `- [ ]` |
| Proportions / numbers over time | Mermaid `pie` or `xychart-beta`; verify rendering in the destination |
| Quick lookup at the end | “决策速查” table: 场景 → 选择 |

## Style and syntax

- Use H2 for sections and H3 for sub-points; number H2s (“1. …”) for reports.
- Keep paragraphs to 3 sentences or fewer. Prefer bullets for parallel items and tables for comparisons across dimensions.
- Use color sparingly and semantically: blue = info, green = recommended, yellow = caution, red = danger, gray = secondary notes. Aim for no more than about 4 colored blocks per page.
- Bold the key phrase of a sentence rather than the entire sentence.
- Table cells contain rich text only. Use Notion Markdown formatting such as `**bold**`, without HTML formatting tags or nested blocks.
- Indent children of callouts, toggles, columns and tabs with actual tabs. For each `<tab>`, put its title on the first indented line, followed by its child content.
- Inside Mermaid, wrap labels containing special characters in double quotes and use `<br>` for line breaks. Preserve code block contents literally.
- Outside code blocks, escape special characters only when they should display literally in prose. Preserve intentional Markdown formatting, links, block tags and attributes. For example, `\*literal\*` displays literal asterisks, while `**key phrase**` remains bold. Use the current spec's escaping rules for literal characters.

## Templates

Adapt these skeletons to the content. Omit empty or irrelevant sections. Localize the title, headings, table headers, callouts, diagram labels and toggle labels into the selected output language; the Chinese labels below are examples, not fixed output text.

**Tech comparison / 选型分析**: TL;DR callout → TOC when applicable → 1. 本质区别 (bullets + Mermaid when useful) → 2. 对比表 → 3. 推荐做法 (numbered list + code when needed) → 4. 坑与技巧 (callouts) → 5. 决策速查表.

**Research / 调研报告**: TL;DR callout → TOC when applicable → 背景与问题 → 关键发现 (each finding = H3 + short paragraph + evidence) → 对比表 or columns when useful → 风险与不确定性 (yellow callout) → 建议与下一步 (to-do list) → 来源 (citations / links, inside a toggle if long) → closing summary.

**Summary of a doc/meeting/article**: TL;DR callout → 要点 (up to 7 bullets) → 细节 (toggles per topic) → Action items (to-do list) → closing summary. Include TOC when applicable.

**Study note / 学习笔记**: 一句话理解 callout → TOC when applicable → 概念图 (Mermaid) → 核心概念 (H3 each) → 例子 (code / tabs) → 常见误区 (red/yellow callout) → 自测问题 (toggles with answers inside) → closing summary.

## Placement

- If the user names a destination page or database, fetch it first and create the page there.
- Otherwise create a private draft, using the tool's explicit draft mode when available. Tell the user it is private and can be moved once they name a destination.

## Verification

### Content and structure

- [ ] TL;DR states the conclusion, not just the topic.
- [ ] The page includes at least one of the visual blocks listed in Required elements, matched to the content.
- [ ] No section is a wall of text longer than about 6 lines; code and structured data are exempt.
- [ ] Code blocks have a language, and block nesting follows the current spec.
- [ ] Icon, title, conditional TOC and closing summary are present.
- [ ] Fetch the created or updated page and inspect the saved content, block types and nesting. Check for truncation and unknown blocks before claiming that the structure passed. Enhanced Markdown tags in fetch output are expected serialization, not evidence that raw syntax is visible to readers.

### Visual rendering

- When browser or app access is available, open the page and inspect the blocks used: Mermaid renders as a diagram without errors, tables show headers and cells, columns are parallel where the viewport permits, tabs switch content, and toggles expand. Check that prose displays intended formatting and literal characters.
- Correct observed problems and recheck the affected blocks. If a block still cannot render, replace it with a simpler supported representation that preserves the meaning and disclose the change.
- Report structural verification and visual verification separately. If the page cannot be viewed or the relevant controls cannot be inspected, mark visual verification as pending and state the concrete limitation. A successful write or fetch alone is insufficient to claim visual verification passed.
