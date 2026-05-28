# Nick Fury Team：Claude Code 角色插件

Nick Fury Team 是一个面向 Claude Code 的角色型插件，把一组可复用的专业角色封装成可安装、可自动触发、也可通过斜杠命令显式调用的技能库。安装后，你可以像调度一个小型专家团队一样使用 Claude：`nick` 负责先接任务、先拷问再编组，尼克负责总体协调，鹰眼负责技术调研，王记者负责信息核验，希尔负责信息采集与情报整理，前端小黑和罗小白分别处理前后端，惩罚者、哨兵负责验收与安全，海螺沉淀知识，公明处理金融研究。它适合技术选型、产品需求、研发落地、设计评审、发布验证、调查归档等复合任务，让 Claude 不只是回答问题，而是按角色边界分工协作。

## 角色一览

| 角色 | slug / 命令 | 定位 | 适合场景 |
|---|---|---|---|
| Nick 入口 | `/nick` | 结构化派单入口 | 先拷问需求、再给 ABC 团队方案、用户选定后再实施 |
| 尼克 | `/nick-fury` | 总控调度 / 战略决策者 | 多角色协作、任务分派、联合调研、整体判断 |
| 鹰眼 | `/yingyan` | 技术调研专家 | 技术选型、框架对比、架构权衡、风险评估 |
| 王记者 | `/wang-jizhe` | 信息搜集员 / 调查记者 | 多源核验、事件调查、证据链、舆情溯源 |
| 池长老 | `/chi-zhanglao` | 统筹协调 / 制作人 | 多项目并行、资源编排、战略规划、复盘 |
| 前端小黑 | `/qianduan-xiaohei` | 前端开发工程师 | UI 实现、组件开发、响应式布局、浏览器验证 |
| 罗小白 | `/luo-xiaobai` | 后端架构工程师 | API 设计、数据库建模、权限、缓存、队列 |
| 惩罚者 | `/chengfazhe` | 测试与验收官 | 测试计划、回归测试、证据优先验收 |
| 弗兰奇 | `/franky` | DevOps / 发布工程师 | CI/CD、GitHub Actions、Pages 发布、部署验证 |
| 伊比喜 | `/yibixi` | 产品经理 / 需求官 | 需求澄清、PRD、用户故事、验收标准 |
| 海螺 | `/hailuo` | 资料整理 / 知识库管理员 | 归档、知识沉淀、流程复用、角色矩阵维护 |
| 希尔 | `/xier` | 信息采集 / 情报执行官 | 监控、线索归档、弱信号捕捉、情报整理 |
| 哨兵 | `/shaobing` | 安全审计员 | 权限、泄密、XSS/SSRF/注入、依赖风险 |
| 蝎 | `/xie` | 自动化工程师 | 浏览器自动化、截图流水线、定时任务、抓取 |
| 胡盘 | `/hupan` | 文案 / 编辑 | 标题提炼、内容压缩、传播优化、报告润色 |
| 转哥 | `/zhuange` | 设计评审 | 视觉分析、布局/配色/间距/字体审查 |
| 公明 | `/gongming` | 全能金融分析师 | 个股研究、组合监控、crypto 尽调、晨会简报 |

## 安装方式

> 下面命令可直接复制到终端执行。安装完成后，重启 Claude Code 或在会话内执行 `/reload-skills`。

### 方式一：通过 Marketplace Source 安装（推荐）

把这个 GitHub 仓库添加为 Claude Code marketplace：

```bash
claude plugin marketplace add https://github.com/ccwq/nick-fury.git --scope user
```

从该 marketplace 安装插件：

```bash
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
```

如果只有一个同名插件，也可以省略 marketplace 名：

```bash
claude plugin install nick-fury-team --scope user
```

刷新技能列表：

```text
/reload-skills
```

查看是否安装成功：

```bash
claude plugin list
claude plugin marketplace list
```

在 `claude plugin list` 中看到 `nick-fury-team@nick-fury-marketplace` 且状态为 `✔ enabled`，即表示插件已加载成功。

### 已添加过 marketplace 时的更新安装

如果你之前已经执行过 `marketplace add`，再次执行会提示：

```text
Marketplace 'nick-fury-marketplace' already on disk — declared in user settings
```

这不是错误，只表示市场源已经存在。但此时本地 marketplace 缓存可能还是旧版本，所以需要先更新缓存，再安装或重装插件：

```bash
claude plugin marketplace update nick-fury-marketplace
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
claude plugin list
```

如果之前安装过失败版本，建议先卸载再重装：

```bash
claude plugin marketplace update nick-fury-marketplace
claude plugin uninstall nick-fury-team --scope user --prune --yes
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
claude plugin list
```

如果卸载时提示 `Plugin "nick-fury-team" not found in installed plugins`，说明当前没有可卸载的已安装实例，可以直接继续执行安装命令。

### 我们这次踩过的坑

你的第一次安装失败，核心原因不是安装命令本身，而是插件市场清单的写法有冲突。

当 `.claude-plugin/marketplace.json` 的插件条目中声明了 `skills`，同时插件目录里又有 `.claude-plugin/plugin.json` 时，Claude Code 会认为 marketplace entry 和 plugin manifest 都在声明组件。若 marketplace entry 使用 `strict: false`，就会报：

```text
Plugin nick-fury-team has conflicting manifests: both plugin.json and marketplace entry specify components.
Set strict: true in marketplace entry or remove component specs from one location.
```

现在已经修复为：

```json
"strict": true
```

这个设置的含义是：以 marketplace entry 中声明的组件为准，允许它和插件自身的 `plugin.json` 共存，不再产生组件声明冲突。

### 另一处命令差异

某些 Claude Code 版本中 `claude plugin details nick-fury-team` 可用，但你的环境返回：

```text
error: unknown command 'details'
```

所以 README 不再把 `details` 作为必需验证命令。统一使用下面两个命令验证：

```bash
claude plugin list
claude plugin marketplace list
```


### 方式二：本地路径安装 / 开发调试

先克隆仓库：

```bash
git clone https://github.com/ccwq/nick-fury.git
cd nick-fury
```

添加本地目录为 marketplace source：

```bash
claude plugin marketplace add . --scope user
```

安装插件：

```bash
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
```

刷新技能列表：

```text
/reload-skills
```

如果只是想在一次 Claude Code 会话里临时加载插件，也可以使用：

```bash
claude --plugin-dir .
```

重新扫描技能后，Claude 会自动加载 `skills/` 下的 17 个技能、`agents/` 下的 15 个原生 subagents，并识别 `commands/` 下的 17 个斜杠命令。

## 卸载方式

### 卸载插件

卸载用户级插件：

```bash
claude plugin uninstall nick-fury-team --scope user
```

如果希望同时清理不再使用的自动依赖：

```bash
claude plugin uninstall nick-fury-team --scope user --prune --yes
```

卸载后刷新技能列表：

```text
/reload-skills
```

### 移除 Marketplace Source

如果是通过 marketplace source 添加的仓库，还可以移除市场源：

```bash
claude plugin marketplace remove nick-fury-marketplace --scope user
```

确认移除结果：

```bash
claude plugin marketplace list
```

## 使用方式

### `/nick`：先拷问、再组队、后实施（推荐）

如果你的任务还比较模糊，或者你希望尼克先把事情接住、先问清楚再安排团队，优先使用 `/nick`：

```text
/nick 帮我把这个需求先对齐一下，再给我一个合适的团队分工方案
```

`/nick` 的工作流是：

1. 先用伊比喜式方式做不超过 3 轮需求拷问（复杂任务可追加）
2. 输出结构化需求摘要
3. 由尼克给出 A / B / C 三个团队方案
4. 你选定方案后，再开始实施

它适合这类场景：

- 你只知道大方向，还没完全想清楚
- 你希望先收敛需求，再决定谁来做
- 你想让尼克先搭团队，而不是直接开干

### 自然语言直接点名尼克

除了 `/nick`，你也可以直接用自然语言点名尼克，例如：

```text
尼克你来完成这个事情
尼克你看看这个需求是不是还要先补几条约束
这个 bug 尼克你处理一下，但先别急着改，先把团队方案给我
```

这类表达会优先命中新的 `nick` skill，先进入“拷问 → 分组 → 选择 → 实施”的流程。

### 自动触发

默认推荐直接用自然语言描述任务，让 Claude 根据技能描述自动选择角色：

```text
帮我做一个前端框架技术选型
```

通常会触发鹰眼。

```text
帮我检查这次改动有没有安全风险
```

通常会触发哨兵。

```text
协调鹰眼和王记者做一次联合调研
```

通常会触发尼克的调度能力。

### 显式命令

如果你想明确指定角色，可以直接使用英文 slug 命令：

```text
/yingyan 对比 React 和 Vue 在中后台项目的选型
/nick 这个事情你先接一下：我要把一个老后台重构上线，但你先帮我问清楚再组团队
/nick-fury 协调鹰眼和王记者做联合调研
/hailuo 把这次调查沉淀成知识库条目
/shaobing 检查当前项目是否有泄密风险
```

显式命令适合以下情况：

- 你明确知道要哪个角色出场
- 自动触发不够稳定
- 多角色任务中需要先指定主负责人
- 想让角色保持固定语气和输出结构

## 仓库结构

```text
.claude-plugin/
  plugin.json          # 插件元数据
  marketplace.json     # 市场源清单
skills/
  <role-slug>/SKILL.md # 17 个技能，承载角色人格与厚能力
agents/
  <role-slug>.md       # 15 个原生 subagent 薄壳，复用对应 skill
commands/
  <role-slug>.md       # 17 个显式调用命令
docs/
  roles.md             # 角色索引与命令对照
```

## 设计原则

1. **角色独立**：每个角色都能单独触发和执行任务。
2. **尼克可调度**：`nick-fury` 既是独立角色，也保留总控协调能力。
3. **Skill 厚能力**：角色人格、工作流、检查清单和输出模板沉淀在 `skills/` 中，供主会话和 subagent 复用。
4. **Agent 薄外壳**：`agents/` 只负责原生 subagent 注册、触发描述、能力边界和最小人格锚点。
5. **自动优先**：日常使用优先依赖自然语言触发。
6. **命令兜底**：需要明确点名时，通过英文 slug 命令调用。
7. **命令分层**：`/nick` 负责先拷问再编组，`/nick-fury` 负责直接总控调度。
8. **保留原稿**：原始编号目录继续保留，作为角色内容来源和迁移对照。

## 原始角色资料

仓库根目录下的 `01-鹰眼/`、`02-王记者/`、`nick-fury/` 等目录是原始角色资料；插件实际加载入口在：

- `skills/`
- `agents/`
- `commands/`
- `.claude-plugin/`

更多角色说明见：`docs/roles.md`。
