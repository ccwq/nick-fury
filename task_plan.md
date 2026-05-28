# Nick Fury 智能调度改造计划

## Current Phase

Phase 3 of 3: 经验固化与后续行为验收准备 — complete

## Phases

| Phase | Name | Status | Notes |
|---|---|---|---|
| 1 | 调查不分派原因 | complete | 确认问题根因是 Nick Fury skill 有角色设定但缺少 Claude Code `Agent` 工具级强规则，且旧文档偏 `delegate_task` 心智。 |
| 2 | 实施智能调度改造 | complete | 已改造 `skills/nick-fury/SKILL.md`、`commands/nick-fury.md`，并补充各角色被分派协议。 |
| 3 | 经验固化与后续行为验收准备 | complete | 已把设计决策、触发阈值、并行/串行策略、验收标准和已改文件沉淀到 PWF 文件。 |

## Core Decisions

- Nick Fury 采用“智能调度器”，不是强制所有任务都派 sub agents。
- 简单任务由 Nick Fury 直接回答；复杂任务必须实际调用 sub agents，而不是只口头说会派谁。
- 复杂度触发阈值：跨 2 个角色、超过 3 步、需要调研、需要实现 + 测试、需要安全/验收独立审查，满足任一即分派。
- 调度方式采用“并行 + 串行混合”：同阶段独立任务并行，后续依赖前序结果的阶段串行。
- 本轮不新增正式 `agents/` 目录，先改 Nick Fury + 各角色 skill，让角色具备被分派和回报的协议。
- 验收以可观察行为为准：简单任务不派；多角色任务派；同阶段多个独立任务并行派；汇总区分“子代理结论”和“尼克判断”。

## Errors Encountered

| Time | Error | Resolution |
|---|---|---|
| 2026-05-27 | 批量插入角色协议时 PowerShell regex `-notmatch '\nick-fury\'` 报非法转义 | 改用 `.Contains()` 字符串判断后重新执行。 |
| 2026-05-27 | 首次按完整段落替换未命中，因文件换行/空行不完全一致 | 改为按锚点行插入“被分派任务协议”。 |

## Next Validation Prompts

1. `/nick-fury 解释一下这个插件是做什么的` → 应直接回答，不调用 Agent。
2. `/nick-fury 协调鹰眼和王记者调研一下 React Server Components 是否适合中后台项目` → 应并行派鹰眼 + 王记者。
3. `/nick-fury 帮我设计并落地一个前后端功能，然后验收` → 应先派需求/前端/后端，完成后再串行派惩罚者验收。
4. `/nick-fury 检查这次改动有没有安全和发布风险` → 应至少派哨兵；涉及发布时派弗兰奇。
