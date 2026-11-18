# Changelog

## v0.2.0 (2026-11-18)

基于「程序员必须亲自掌握数据库 schema 与关键技术选型」的技术审计（ChatGPT/Claude/Gemini 三路交叉 + 原文核验）升级：

### 破坏性变更
- **L1 事实层拆分**：`design.md` 一拆四——`design.md`（架构总览与边界）、`schema.md`（数据模式当前态，S 编号）、`stack.md`（技术栈当前态）、`decisions.md`（技术决策记录 ADR，D 编号）。默认骨架由八文件变十文件；存量项目升级走 `/gsdd-change` 提案通道（变更必录 change-log）。

### 新增
- **编号引用链扩为 R→S→A→剧本→tests**：schema 条目编号 S、注明支撑的 R 号；D 号挂接组件与决策；`/gsdd-audit` 增加三向对账（有 R 无 A / 有 A 无测试 / S 支撑的 R 不存在）。
- **单向门治理**：decisions.md 模板带可逆性标记；`/gsdd-change` 对单向门条目（数据库引擎、云平台、鉴权、破坏性迁移）强制必填替代方案/回退成本/批准人，缺项不得采纳；`/gsdd-audit` 增加单向门完备性检查。
- **迁移对账**：schema.md 条目登记当前迁移编号；`/gsdd-audit` 与代码库 migration 目录只读启发式对账。
- **/gsdd-load 数据事实附加**：改表/换库/迁移类任务简报强制附 schema 条目、stack 组件、decisions 相关 D 号，缺失标红。
- **tests/ 引用约定**：`/gsdd-init` 创建/补充 `tests/README.md`，约定测试标注 A 编号的格式，作为编号对账依据。

### 修复
- **内部矛盾**：`/gsdd-audit` 原「恰好八文件+README，无多余文件」与渐进披露域拆分（`<名>-<域>.md`）冲突 → 改为「入口文件齐备 + 拆分文件被入口索引登记 + 孤儿文件报警」。
- **entry-pointer 硬编码 300 行阈值** → 改为「以 ssot/README.md 头部声明为准」（默认仍 300）。
