# Nick Fury Team：Claude Code 角色插件

Nick Fury Team 是一个面向 Claude Code 的角色型插件。它把一组可复用的专业角色封装成可安装、可自动触发、也可通过斜杠命令显式调用的技能库。

安装后，你可以像调度一个小型专家团队一样使用 Claude：

- `nick`：结构化入口，先接任务、先拷问、再编组。
- `nick-fury`：总控协调，负责多角色调度与整体判断。
- `yingyan`、`wang-jizhe`、`qianduan-xiaohei`、`luo-xiaobai` 等角色：分别覆盖技术调研、信息核验、前端、后端、测试、安全、设计、自动化、知识沉淀、金融研究等场景。

适合用于技术选型、产品需求、研发落地、设计评审、发布验证、调查归档等复合任务，让 Claude 不只是回答问题，而是按角色边界分工协作。

## 目录

- [角色一览](#角色一览)
- [快速安装](#快速安装)
- [更新与重装](#更新与重装)
- [卸载与清理](#卸载与清理)
- [使用方式](#使用方式)
- [常见问题](#常见问题)
- [仓库结构](#仓库结构)
- [设计原则](#设计原则)
- [原始角色资料](#原始角色资料)

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

## 快速安装

> 下面命令可直接复制到终端执行。安装完成后，重启 Claude Code，或在会话内执行 `/reload-skills`。

### 方式一：通过 GitHub Marketplace Source 安装（推荐）

添加 marketplace source：

```bash
claude plugin marketplace add https://github.com/ccwq/nick-fury.git --scope user
```

安装插件：

```bash
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
```

刷新技能列表：

```text
/reload-skills
```

验证安装结果：

```bash
claude plugin list
claude plugin marketplace list
```

如果 `claude plugin list` 中出现 `nick-fury-team@nick-fury-marketplace`，且状态为 `✔ enabled`，说明插件已加载成功。

### 方式二：本地路径安装 / 开发调试

先克隆仓库：

```bash
git clone https://github.com/ccwq/nick-fury.git
cd nick-fury
```

把当前目录添加为 marketplace source：

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

临时加载不会等同于正式安装；它更适合开发调试。

## 更新与重装

### marketplace 已存在时

如果你再次执行 `marketplace add` 时看到：

```text
Marketplace 'nick-fury-marketplace' already on disk — declared in user settings
```

这不是错误，只表示 marketplace source 已登记。它**不代表插件已安装成功**，也**不代表本地 marketplace 索引已经是最新**。

此时建议先更新 marketplace，再安装插件：

```bash
claude plugin marketplace update nick-fury-marketplace
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
claude plugin list
```

### 安装失败后重装

如果之前安装失败，建议使用最兼容的基础重装流程：

```bash
claude plugin marketplace update nick-fury-marketplace
claude plugin uninstall nick-fury-team --scope user
claude plugin install nick-fury-team@nick-fury-marketplace --scope user
claude plugin list
```

如果卸载时提示：

```text
Plugin "nick-fury-team" not found in installed plugins
```

说明当前没有可卸载的已安装实例，可以继续执行后面的安装命令。

### 可选：清理不再使用的自动依赖

部分 Claude CLI 版本支持 `--prune -y`。如果你的本机支持，并且你希望清理不再使用的自动依赖，可以执行：

```bash
claude plugin uninstall nick-fury-team --scope user --prune -y
```

如果遇到 `unknown option`，以本机帮助为准：

```bash
claude plugin uninstall --help
```

## 卸载与清理

### 只卸载插件

```bash
claude plugin uninstall nick-fury-team --scope user
```

卸载后刷新技能列表：

```text
/reload-skills
```

确认插件已卸载：

```bash
claude plugin list
```

如果输出中已经看不到 `nick-fury-team@nick-fury-marketplace`，说明插件已卸载成功。

### 卸载插件并移除 marketplace source

```bash
claude plugin uninstall nick-fury-team --scope user
claude plugin marketplace remove nick-fury-marketplace
```

确认移除结果：

```bash
claude plugin list
claude plugin marketplace list
```

如果 `claude plugin marketplace list` 中已经看不到 `nick-fury-marketplace`，说明 marketplace source 已移除成功。

> `claude plugin marketplace remove nick-fury-marketplace` 不带 `--scope` 时，会尝试从所有 settings scope 移除该 marketplace。若你只想移除用户级配置，并且你的 CLI 支持该参数，可以使用：
>
> ```bash
> claude plugin marketplace remove nick-fury-marketplace --scope user
> ```
>
> 如遇参数兼容问题，请以本机帮助输出为准：`claude plugin marketplace remove --help`。

### 删除本地克隆仓库

`plugin uninstall` 和 `marketplace remove` 只会清理 Claude Code 中的插件和 marketplace source，**不会自动删除你磁盘上的 Git 仓库目录**。

如果你还想删除本地克隆目录，请先确认里面没有自己的未提交修改，再退出该目录并删除文件夹。例如 PowerShell：

```powershell
cd ..
Remove-Item -Recurse -Force .\nick-fury
```

也可以直接在 Windows 资源管理器中删除 `nick-fury` 文件夹。

> 注意：删除本地仓库目录是不可逆操作。删除前请确认没有需要保留的文件。

### 卸载、移除、删除目录的区别

| 操作 | 命令 / 方式 | 影响 |
|---|---|---|
| 卸载插件 | `claude plugin uninstall nick-fury-team --scope user` | 停用 Claude Code 中的插件 |
| 移除 marketplace source | `claude plugin marketplace remove nick-fury-marketplace` | 移除 marketplace 登记来源 |
| 删除本地仓库 | `Remove-Item -Recurse -Force .\nick-fury` 或手动删除文件夹 | 删除你电脑上的 clone 目录 |

## 使用方式

### `/nick`：先拷问、再组队、后实施（推荐）

如果你的任务还比较模糊，或者你希望尼克先把事情接住、先问清楚再安排团队，优先使用 `/nick`：

```text
/nick 帮我把这个需求先对齐一下，再给我一个合适的团队分工方案
```

`/nick` 是团队协议入口，不是快捷执行命令。它的工作流是：

1. 先用伊比喜式方式做不超过 3 轮需求拷问（复杂任务可追加）
2. 输出结构化需求摘要
3. 由尼克给出 A / B / C 三个团队方案
4. 你选定方案后，进入真实团队调度，而不是由单个 assistant 假装分工执行

如果当前环境不能启动真实团队调度，`/nick` 会停止并说明原因，不会口头编队后继续单人执行。

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

这类表达会优先命中新的 `nick` skill，先进入“拷问 → 分组 → 选择 → 真实团队调度”的流程。

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

`/nick-fury` 是直接总控 / 协调入口，不走 `/nick` 的 A/B/C 入口闸门；不想先走拷问和选队时用 `/nick-fury`，想先澄清并选择团队方案时用 `/nick`。

显式命令适合以下情况：

- 你明确知道要哪个角色出场
- 自动触发不够稳定
- 多角色任务中需要先指定主负责人
- 想让角色保持固定语气和输出结构

## 常见问题

### `Plugin "nick-fury-team" not found in marketplace "nick-fury-marketplace"`

如果你严格按 README 执行，仍然遇到这个错误，优先排查下面两类原因：

1. **本地 marketplace 索引未刷新**

   `marketplace add` 成功或提示已存在，只代表来源已登记，不代表本地缓存一定是最新。先执行：

   ```bash
   claude plugin marketplace update nick-fury-marketplace
   ```

2. **远端仓库内容与本地工作区不一致**

   Claude 安装时读取的是远端 GitHub 仓库的 marketplace 清单，而不是你当前本地目录。如果你本地已经修过 `.claude-plugin/marketplace.json`，但还没 push 到远端，安装时依然会读到旧清单。

当前仓库中的定义关系如下：

- marketplace 名来自 `.claude-plugin/marketplace.json` 的 `name`，当前值为 `nick-fury-marketplace`
- 插件安装名来自 `.claude-plugin/marketplace.json` 中 `plugins[].name`，当前值为 `nick-fury-team`
- 插件自身 manifest 位于 `.claude-plugin/plugin.json`，当前 `name` 也是 `nick-fury-team`

因此，当前推荐的安装命令本身没有问题；如果报 `not found`，通常是 marketplace 索引状态问题，而不是命令拼写错误。

### `Plugin nick-fury-team has conflicting manifests`

如果看到：

```text
Plugin nick-fury-team has conflicting manifests: both plugin.json and marketplace entry specify components.
Set strict: true in marketplace entry or remove component specs from one location.
```

这与 `Plugin not found in marketplace` 不是同一个问题。

它只会出现在较早的 marketplace 配置中：当 `.claude-plugin/marketplace.json` 的插件条目声明了组件，同时插件目录里又存在 `.claude-plugin/plugin.json` 时，如果 marketplace entry 没有正确使用 `strict: true`，Claude Code 会把两处组件声明视为冲突。

当前仓库已经修复为：

```json
"strict": true
```

也就是说：如果你现在遇到的是 `not found in marketplace`，请优先排查缓存或远端仓库内容，而不是把它和 manifest 冲突混为一谈。

### `error: unknown command 'details'`

某些 Claude Code 版本中 `claude plugin details nick-fury-team` 可用，但部分环境会返回：

```text
error: unknown command 'details'
```

因此 README 不把 `details` 作为必需验证命令。统一使用：

```bash
claude plugin list
claude plugin marketplace list
```

### `unknown option`

不同 Claude CLI 版本的参数支持存在差异。如果遇到 `unknown option`，请先查看本机命令帮助：

```bash
claude plugin install --help
claude plugin uninstall --help
claude plugin marketplace add --help
claude plugin marketplace remove --help
claude plugin marketplace update --help
```

README 中优先使用基础命令；涉及 `--prune -y`、`marketplace remove --scope user` 等参数时，请以本机帮助输出为准。

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
7. **命令分层**：`/nick` 负责先拷问再编组，用户选定后进入真实 Agent-first 调度；`/nick-fury` 负责直接总控调度。
8. **保留原稿**：原始编号目录继续保留，作为角色内容来源和迁移对照。

## 原始角色资料

仓库根目录下的 `01-鹰眼/`、`02-王记者/`、`nick-fury/` 等目录是原始角色资料；插件实际加载入口在：

- `skills/`
- `agents/`
- `commands/`
- `.claude-plugin/`

更多角色说明见：`docs/roles.md`。
