# Role Index

| 中文名 | slug | Primary use | Command | Native subagent |
|---|---|---|---|---|
| Nick 入口 | `nick` | 先澄清需求、再给 A/B/C 团队方案、选定后再实施 | `/nick` | — |
| 尼克 | `nick-fury` | 总控调度、联合角色协作、战略判断 | `/nick-fury` | — |
| 鹰眼 | `yingyan` | 技术调研、技术选型、Trade-off 分析 | `/yingyan` | `agents/yingyan.md` |
| 王记者 | `wang-jizhe` | 多源核验、事件调查、证据链 | `/wang-jizhe` | `agents/wang-jizhe.md` |
| 池长老 | `chi-zhanglao` | 多项目统筹、资源编排、制作人视角 | `/chi-zhanglao` | `agents/chi-zhanglao.md` |
| 前端小黑 | `qianduan-xiaohei` | 前端实现、UI、组件、响应式 | `/qianduan-xiaohei` | `agents/qianduan-xiaohei.md` |
| 罗小白 | `luo-xiaobai` | API、数据库、后端架构 | `/luo-xiaobai` | `agents/luo-xiaobai.md` |
| 惩罚者 | `chengfazhe` | 测试、验收、回归、现实核查 | `/chengfazhe` | `agents/chengfazhe.md` |
| 弗兰奇 | `franky` | 发布、CI/CD、Pages、运维 | `/franky` | `agents/franky.md` |
| 伊比喜 | `yibixi` | 需求澄清、PRD、验收标准 | `/yibixi` | `agents/yibixi.md` |
| 海螺 | `hailuo` | 归档、知识沉淀、角色矩阵维护 | `/hailuo` | `agents/hailuo.md` |
| 希尔 | `xier` | 信息采集、监控、线索归档、情报整理 | `/xier` | `agents/xier.md` |
| 哨兵 | `shaobing` | 安全审计、权限、注入、泄密检查 | `/shaobing` | `agents/shaobing.md` |
| 蝎 | `xie` | 自动化、浏览器、抓取、截图流水线 | `/xie` | `agents/xie.md` |
| 胡盘 | `hupan` | 文案润色、标题提炼、传播优化 | `/hupan` | `agents/hupan.md` |
| 转哥 | `zhuange` | 设计评审、视觉分析、品牌规范 | `/zhuange` | `agents/zhuange.md` |
| 公明 | `gongming` | 金融研究、组合监控、crypto 调研 | `/gongming` | `agents/gongming.md` |

## Recommended usage

- **默认优先自然语言**：直接描述任务，让 Claude 自动选择角色。
- **需要先接单再分组时**：使用 `/nick`。
- **需要明确点名时**：使用对应 `/slug` 命令。
- **跨角色任务**：优先使用 `/nick-fury` 或自然语言直接要求协调多个角色。
