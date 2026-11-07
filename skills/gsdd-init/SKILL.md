---
name: gsdd-init
description: 为项目创建 GSDD SSOT 骨架：先生成中性八文件并接线编程工具入口（CLAUDE.md/AGENTS.md 等），再问答式收窄填充内容。
disable-model-invocation: true
---

# GSDD 初始化

GSDD（Governed Spec-Driven Development）：程序员盯 SSOT，不盯代码。本命令为当前项目建立 SSOT 骨架并接入日常工具链。目标项目根：args 给了就用，否则当前工作区。

## 前置判定

- `ssot/` 已存在 → **跳过骨架生成，只做第二步入口接线**（绝不覆盖现有 ssot/），收尾时建议人跑 `/gsdd-audit`。
- `ssot/` 不存在 → 全流程。

## 第一步：中性骨架（不预填任何项目内容）

把本 skill 目录下 `templates/ssot/` 整目录复制为 `<项目根>/ssot/`：

```text
ssot/
├── README.md                          # 治理规则头部 + 文件清单 + 渐进披露阈值
├── L0-requirements/{product,requirements,change-log}.md
├── L1-facts/{glossary,design}.md
└── L2-process/{testcase,acceptance}.md
```

中性 = 只有职责声明、索引段和空结构，不含任何假设的项目内容。骨架先落盘——访谈中断也不损失，下次继续即可。

## 第二步：入口接线（幂等）

1. 检测项目根的编程工具入口文件（存在的才算）：`CLAUDE.md`、`AGENTS.md`、`GEMINI.md`、`.cursorrules`、`.cursor/rules`、`.github/copilot-instructions.md`。
2. 一个都没有 → 创建 `CLAUDE.md`。
3. 对每个入口文件：把本 skill 目录下 `templates/entry-pointer.md` 的 GSDD 段**追加**进去（绝不覆盖已有内容）；文件里已含「GSDD」字样的跳过，不重复添加。

## 第三步：问答式收窄（AI 只起草，用户采纳才写入 ssot/）

逐层访谈，每轮先给草稿、用户确认后落盘：

1. **L0 product.md**：用户是谁、核心价值、范围边界——重点逼问「不做什么」。
2. **L0 requirements.md**：逐条编号 R1…；只写现行版本，历史留给 change-log。
3. **L1 glossary.md**：从前两步草稿提候选术语，逐条收窄口径；一条一个定义。
4. **L1 design.md**：架构、选型、存储格式、关键配置；字段必须能映射到术语表。
5. **L2 acceptance.md**：验收项编号 A1…；写行为不写代码；TDD 收窄口径（业务规则必须测，UI 文案与样式不进自动化）。
6. **L2 testcase.md**：剧本每步只引用 A 号，不定义新标准。
7. **L0 change-log.md**：追加首条基线条目（日期、初始需求、波及文件=全部八文件）。

每层落盘时同步维护该文件的 `## 索引` 段（一行一条目：编号|标题|关键词）。

## 收尾

自检：八文件齐备、索引段存在、编号无重号。提醒用户：以后需求变更走 `/gsdd-change`，交付前走 `/gsdd-accept`，定期跑 `/gsdd-audit`。
