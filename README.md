# NanSsye-de Agent 工作台

> 多 Agent 协作系统实战记录 — 基于 OpenClaw 三角架构

## 系统架构

```
┌─────────────────────────────────────────┐
│               main (总管)                │
│  接收指令 → 判断类型 → 派发任务 → 汇总输出 │
└──────────┬──────────────────────────────┘
           │ sessions_spawn
     ┌─────┴─────┐
     ▼           ▼
┌─────────┐  ┌──────────┐
│ executor │  │ reviewer │
│  执行者  │  │   审查   │
└─────────┘  └──────────┘
```

## 三角架构

| 角色 | 职责 | 特点 |
|------|------|------|
| **main** | 接收指令、判断、派发、汇总 | 不做底层实操 |
| **executor** | 埋头干活、改文件、跑命令 | 只执行不调度 |
| **reviewer** | 复核高风险操作结果 | 安全守门人 |

## 派发规则

```
sessions_spawn(
  task="具体任务描述",
  agentId="executor" 或 "reviewer",
  runtime="subagent",
  mode="run",
  runTimeoutSeconds=任务预计耗时
)
```

- **自己答**：纯知识问答、解释、简单建议
- **派给执行**：改文件、跑命令、查日志、调服务、代码编写
- **派给审查**：删除、迁移、systemd、权限变更、核心配置

## 记忆系统

三层自学习体系：
- **self-evolve**：运行时收集用户评分，生成学习记忆
- **self-improving**：错误记录、最佳实践、知识纠正
- **proactivity**：主动预见需求，保持工作推进

## 能力方向

- 多 Agent 协作编排
- GitHub 自动化（repo / PR / actions）
- 记忆系统与长期上下文管理
- 语音与微信消息链路
- Skill 工程化与复用

## 公开 Skill 仓库

NanSsye-de 将以下 Skill 整理为独立开源仓库，可直接安装使用：

| 仓库 | 说明 |
|------|------|
| [`NanSsye-de/skill-creator`](https://github.com/NanSsye-de/skill-creator) | OpenClaw Skill 开发工程化指南，从构思到发布的完整工作流 |
| [`NanSsye-de/news-aggregator-skill`](https://github.com/NanSsye-de/news-aggregator-skill) | 28源新闻聚合Skill（Hacker News / GitHub / 微信公众号 / 微博等） |
| [`NanSsye-de/jayson-wx-sum`](https://github.com/NanSsye-de/jayson-wx-sum) | 微信公众号文章抓取摘要，无需浏览器/Playwright |
| [`NanSsye-de/amap-maps`](https://github.com/NanSsye-de/amap-maps) | 高德地图MCP插件（路径规划/地理编码/天气查询） |

> 更多仓库整理中，完整列表见 [NanSsye-de 所有公开仓库](https://github.com/NanSsye-de?tab=repositories)
