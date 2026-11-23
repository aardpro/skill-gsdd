# skill-gsdd

把 GSDD（Governed Spec-Driven Development，治理规范驱动开发）方法论做成的一套 skill。
核心主张：**程序员盯 SSOT，不盯代码。**

- 方法论装在 skill（通用、跨项目），项目实例装在各项目的 `ssot/`（三层分区，默认十文件，项目内维护）。
- AI 对 `ssot/` 全层只读；一切规范变更走 `/gsdd-change` 提案通道，人采纳才落盘。

## 命令

| 命令 | 干什么 | 可写 ssot/? |
|---|---|---|
| `/gsdd-init` | 中性骨架（三层十文件）→ 图谱防护文件（`.graphifyignore`/`.cbmignore`）→ 编程工具入口接线（CLAUDE.md/AGENTS.md…）→ 问答式收窄 | 采纳后 |
| `/gsdd-load` | **开始新任务**：索引先行、按需取段，产出编号级任务简报，判定下一步——直接实现还是走 `/gsdd-change` | 否 |
| `/gsdd-change` | **规范要变时**（load/accept 发现缺口后走的岔路）：变更提案 change-log 条目 + 波及 diff；采纳后按锁死顺序落盘 | 采纳后 |
| `/gsdd-accept` | 沿剧本按验收项编号走查/跑测试，证据归档 `evidence/` | 否 |
| `/gsdd-audit` | 体检：结构 / 编号通货 / 索引一致性 / 只追加 / 规模预警 | 否（只报告） |

使用节奏：任务开始一律先 `/gsdd-load`——规范变没变由简报判断，不靠记忆：任务已被 R/A 号覆盖 → 直接实现 → `/gsdd-accept`；简报暴露缺口（需求没登记 / 验收没覆盖 / 术语缺口径）→ `/gsdd-change` → 采纳 → 再 `/gsdd-load` → 实现 → `/gsdd-accept`。accept 走查再发现缺口也汇入 `/gsdd-change`；`/gsdd-audit` 定期体检。

## 渐进披露（四级）

1. **入口最小化**：工具入口文件只指向 `ssot/README.md`（路由器），永不"读整个 ssot/"。
2. **索引先行**：每文件头部 `## 索引`（一行一条条目：编号|标题|关键词）；索引只引用、不定义。
3. **读取纪律**：层文件 ≤300 行可全文读；超过必须 grep 定位后按行号取段；任务简报默认 ≤2000 token（约 120 行，按行数判定），超限按锁死优先级压缩且开头标红，不许静默。
4. **域拆分阀**：>400 行按域拆 `<名>-<域>.md`，原文件退化为纯索引；入口文件集不变，拆分文件必须在入口索引登记。

阈值在 `ssot/README.md` 头部声明，项目可调。

## 图谱防护

`/gsdd-init` 会在项目根写入 `.graphifyignore` 与 `.cbmignore`（两文件内容相同，幂等只补缺失行，条目全清单见模板）：GSDD 规范防护段声明 `ssot/`、`evidence/` 不进伴侣图谱工具（graphify / codebase-memory-mcp）的索引——图谱是索引不是事实来源，规范目录不得被吸入；另带构建产物、本地临时产物、AI 编程工具与 IDE 会话/状态目录（`.zcode/`、`.claude/`、`.codex/`、`.opencode/`、`.trae/`、`.qoder/`、`.cursor/`、`.idea/`、`.vscode/` 等）、图谱工具自身产物等排除段，图谱只保留源码与有效文档。文件与是否已安装图谱工具无关：防护先落地，后装工具也受保护。`/gsdd-audit` 会对"存在图谱工具痕迹但缺防护文件"的项目给出提示级报告。

## 布局

`/gsdd-init` 在目标项目新建的文件与功能（中性骨架先落盘，内容靠问答式收窄逐步填充）：

```text
<项目根>/
├── ssot/                     # 唯一事实来源（AI 只读，变更走 /gsdd-change）：三层十文件 + 路由器
│   ├── README.md             # 路由器：治理规则头部 + 文件清单 + 阈值声明；工具入口只指向它
│   ├── L0-requirements/      # 做什么
│   │   ├── product.md        # 用户、核心价值、范围边界（逼问「不做什么」）
│   │   ├── requirements.md   # 现行需求，编号 R1…；历史只进 change-log
│   │   └── change-log.md     # 只追加的变更历史：日期、需求、波及文件
│   ├── L1-facts/             # 是什么
│   │   ├── glossary.md       # 术语口径，一条一个定义
│   │   ├── design.md         # 架构总览与边界：组件关系、数据流、关键配置
│   │   ├── schema.md         # 实体/字段/约束，编号 S1…，每条映射 R 号与术语
│   │   ├── stack.md          # 组件、版本、运行约束，关键选型引用 D 号
│   │   └── decisions.md      # 关键决策，编号 D1…：背景/候选/理由/可逆性
│   └── L2-process/           # 怎么验收
│       ├── acceptance.md     # 验收项，编号 A1…，写行为不写代码
│       └── testcase.md       # 测试剧本，每步只引用 A 号，不定义新标准
├── CLAUDE.md 等入口文件       # 追加 GSDD 段指向 ssot/README.md（一个入口都没有才新建 CLAUDE.md）
├── tests/README.md           # 测试标注 A 编号的引用约定，/gsdd-audit 编号对账依据
├── .graphifyignore           # 图谱防护文件（详见「图谱防护」节）
└── .cbmignore                # 同上，两文件内容相同
```

入口接线只追加不覆盖：检测 `CLAUDE.md`、`AGENTS.md`、`GEMINI.md`、`.cursorrules`、`.cursor/rules`、`.github/copilot-instructions.md`，已含 GSDD 段的跳过；已有 `tests/` 时只补 `README.md`，不动现有测试。

## 安装

任选其一（都装五个完整 skill 目录，含 `templates/`）：

```bash
# ① skills CLI（需 Node ≥ 22.20；支持 ZCode、Claude Code、Cursor 等 75+ agent）
npx skills add aardpro/skill-gsdd -a zcode -g          # ZCode 全局 → ~/.zcode/skills/
npx skills add aardpro/skill-gsdd -a claude-code -g    # Claude Code 全局 → ~/.claude/skills/
npx skills add aardpro/skill-gsdd -g                   # 交互式自选 agent 与范围

# ② 手动复制
git clone https://github.com/aardpro/skill-gsdd.git
cp -r skill-gsdd/skills/gsdd-* ~/.zcode/skills/
```

装完重启会话生效；五个命令均为用户手动触发（`disable-model-invocation: true`），首次使用在目标项目跑 `/gsdd-init`。

升级：`npx skills update`（或重新 ①/② 覆盖）。
