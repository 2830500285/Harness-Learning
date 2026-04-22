# Harness Learning Design

Date: 2026-04-22

## Goal

Build a public GitHub knowledge base that organizes high-signal material about AI agent harness design from major companies and respected practitioners.

构建一个公开的 GitHub 知识库，系统整理 AI agent harness 设计相关的高质量资料，来源包括主流公司与一线实践者。

## Working Definition

For this repository, "harness" is the practical infrastructure around an LLM:

- runtime
- tool loop
- context management
- artifact handoff
- UI or app integration
- telemetry and traces
- evaluation and iteration

## Content Strategy

Use a layered structure:

1. Entry layer: explain the topic and give reading paths.
2. Source layer: catalog authoritative articles with dates and links.
3. Synthesis layer: summarize company and expert perspectives.

采用三层结构：

1. 入口层：解释主题并给出阅读路径。
2. 来源层：整理权威文章、日期与链接。
3. 综合层：提炼公司与专家的核心观点。

## Initial Source Mix

Companies:

- Anthropic
- OpenAI

Practitioners:

- Simon Willison
- Hamel Husain
- Eugene Yan
- Lilian Weng

## File Structure

- `README.md`
- `knowledge-base/index.md`
- `knowledge-base/sources.md`
- `knowledge-base/companies/anthropic.md`
- `knowledge-base/companies/openai.md`
- `knowledge-base/experts/lilian-weng.md`
- `knowledge-base/experts/simon-willison.md`
- `knowledge-base/experts/hamel-husain.md`
- `knowledge-base/experts/eugene-yan.md`

## Design Choices

- Bilingual by default: every major note includes English and Chinese.
- Summary-first: no raw article dumps.
- Actionable notes: each source ends with practical implications.
- Expandable structure: new companies and experts can be added without reworking the tree.

## Non-Goals For V0.1

- full-text archiving
- automated syncing
- PDF ingestion pipeline
- tagging system beyond simple folders

## Success Criteria

- The repo is public.
- The structure is easy to browse.
- Each source note clearly states what the article says and why it matters.
- A reader can form a solid mental model of agent harness design after one sitting.
