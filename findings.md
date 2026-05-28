# Findings

## Nick Fury 不主动分派的根因

- `skills/nick-fury/SKILL.md` 原本写了“不直接执行细节任务”，但没有把它落成 Claude Code 可执行的 `Agent` 工具协议。
- 插件历史内容里存在 `delegate_task` 心智，但当前 Claude Code 可用的是 `Agent` 工具；这会让模型知道“该委派”，却不知道“现在要实际调用哪个工具”。
- 原有 Nick Fury 内容更像角色设定/世界观，不是硬性操作协议；缺少必须分派条件、并行派发规则、窄任务模板和汇总格式。
- 项目此前没有正式 `agents/` 目录，因此首次改造先落在 skill 协议；本轮已按 Claude Code 规范补充 `agents/` 原生 subagent 薄外壳。

## 已确认用户偏好

- Nick Fury 应是智能调度器：小任务自己处理，大任务分派。
- 分派触发基于复杂度阈值，而不是只靠关键词。
- 调度策略使用并行 + 串行混合。
- 本轮范围升级为 `Skill 厚能力 + Agent 薄外壳`，避免 agents 与 skills 双份维护。
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

- `agents/*.md`
  - 新增 15 个正式执行角色的原生 subagent 薄外壳。
  - 每个 agent 指向对应 skill，保留最小人格锚点、能力边界和统一回报格式。

- `.claude-plugin/plugin.json` / `.claude-plugin/marketplace.json`
  - 显式声明 commands、agents、skills 组件。
  - 补齐 `./skills/nick`，并更新插件描述为 17 skills + 15 native subagents。

- `README.md` / `docs/roles.md`
  - 更新仓库结构、设计原则和角色索引，标明 native subagent 对应关系。

## 静态检查结果

- `skills/nick-fury/SKILL.md` 已包含：智能调度协议、必须分派条件、Claude Code 分派方式、原生 agents 优先策略、汇总格式、行为验收标准。
- 15 个非 Nick Fury 角色 skill 已包含“被分派任务协议”。
- 15 个 `agents/*.md` 已创建，采用“agent 薄外壳 + skill 厚能力”模式。
- `commands/nick-fury.md` 已包含 intelligent orchestration / native subagent fallback 指令。
