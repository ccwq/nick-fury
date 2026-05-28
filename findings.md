# Findings

## Nick Fury 不主动分派的根因

- `skills/nick-fury/SKILL.md` 原本写了“不直接执行细节任务”，但没有把它落成 Claude Code 可执行的 `Agent` 工具协议。
- 插件历史内容里存在 `delegate_task` 心智，但当前 Claude Code 可用的是 `Agent` 工具；这会让模型知道“该委派”，却不知道“现在要实际调用哪个工具”。
- 原有 Nick Fury 内容更像角色设定/世界观，不是硬性操作协议；缺少必须分派条件、并行派发规则、窄任务模板和汇总格式。
- 项目当前没有新增正式 `agents/` 目录，因此本轮选择先改 skill 协议，不扩大到完整 subagent definitions。

## 已确认用户偏好

- Nick Fury 应是智能调度器：小任务自己处理，大任务分派。
- 分派触发基于复杂度阈值，而不是只靠关键词。
- 调度策略使用并行 + 串行混合。
- 本轮范围是 Nick Fury + 各角色 skill，不先重构完整插件结构。
- 成功标准是行为验收，而不是只看文件是否写完。

## 已改关键文件

- `skills/nick-fury/SKILL.md`
  - 新增智能调度协议。
  - 写入不分派/必须分派条件。
  - 写入 Claude Code `Agent` 分派方式。
  - 写入并行/串行组合、prompt 模板、汇总格式和行为验收标准。

- `commands/nick-fury.md`
  - 将“说哪个角色应该协作”改为满足条件时实际使用 sub agent tooling。

- `skills/*/SKILL.md`
  - 为 15 个角色补充“被分派任务协议”和回报格式。
  - `gongming`、`xier` 因原文件结构不同，单独插入定制版协议。

## 静态检查结果

- `skills/nick-fury/SKILL.md` 已包含：智能调度协议、必须分派条件、Claude Code 分派方式、汇总格式、行为验收标准。
- 15 个非 Nick Fury 角色 skill 已包含“被分派任务协议”。
- `commands/nick-fury.md` 已包含 intelligent orchestration / sub agent tooling 指令。
