# skill-gsdd

把 GSDD（Governed Spec-Driven Development，治理规范驱动开发）方法论做成的一套 skill。
核心主张：**程序员盯 SSOT，不盯代码。**

- 方法论装在 skill（通用、跨项目），项目实例装在各项目的 `ssot/`（三层八文件，项目内维护）。
- AI 对 `ssot/` 全层只读；一切规范变更走 `/gsdd-change` 提案通道，人采纳才落盘。

## 命令

| 命令 | 干什么 | 可写 ssot/? |
|---|---|---|
| `/gsdd-init` | 中性八文件骨架 → 编程工具入口接线（CLAUDE.md/AGENTS.md…）→ 问答式收窄 | 采纳后 |
| `/gsdd-load` | 索引先行、按需取段，产出编号级任务简报（≤2000 token） | 否 |
| `/gsdd-change` | 变更提案：change-log 条目 + 波及 diff；采纳后按锁死顺序落盘 | 采纳后 |
| `/gsdd-accept` | 沿剧本按验收项编号走查/跑测试，证据归档 `evidence/` | 否 |
| `/gsdd-audit` | 体检：结构 / 编号通货 / 索引一致性 / 只追加 / 规模预警 | 否（只报告） |

## 渐进披露（四级）

1. **入口最小化**：工具入口文件只指向 `ssot/README.md`（路由器），永不"读整个 ssot/"。
2. **索引先行**：每文件头部 `## 索引`（一行一条条目：编号|标题|关键词）；索引只引用、不定义。
3. **读取纪律**：层文件 ≤300 行可全文读；超过必须 grep 定位后按行号取段；任务简报 ≤2000 token。
4. **域拆分阀**：>400 行按域拆 `<名>-<域>.md`，原文件退化为纯索引；八个入口不变。

阈值在 `ssot/README.md` 头部声明，项目可调。

## 布局

```text
skills/                        # skills CLI 的规范容器（勿改名，见安装①）
├── gsdd-init/                 # SKILL.md + templates/（ssot 八文件中性模板 + entry-pointer.md）
├── gsdd-load/
├── gsdd-change/
├── gsdd-accept/
└── gsdd-audit/
```

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
