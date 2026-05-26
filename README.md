# Doc-Driven Development (DDD) — Claude Code Plugin

一个基于**文档驱动开发**理念的 Claude Code 技能插件。

## 核心原则

> **先文档，后代码。**

任何代码变更前，必须检查并更新相关文档。区别只在于流程的详略程度：

| 模式 | 适用场景 | 流程 |
|------|----------|------|
| **完整 DDD** | 新功能、架构调整、接口变更 | 需求 → 用户确认 → 设计 → 用户确认 → 实现 → 校验 |
| **轻量 DDD** | Bug 修复、小改动、配置调整 | 审查文档 → 同步更新 → 实现 |
| **仅同步文档** | 用户明确跳过流程、极微小改动 | 修复过时文档 → 实现 |

## 安装

将 marketplace 添加到 Claude Code：

```sh
claude plugin marketplace add BoltDoggy/doc-driven-development.skills
```

或通过 `.claude/settings.json` 配置：

```json
{
  "extraKnownMarketplaces": {
    "doc-driven-development": {
      "source": { "source": "github", "repo": "BoltDoggy/doc-driven-development.skills" }
    }
  },
  "enabledPlugins": {
    "doc-driven-development@doc-driven-development": true
  }
}
```

## 目录结构

```
.
├── .claude/
│   ├── plugin.json          # 插件清单
│   └── marketplace.json     # 市场注册信息
├── skills/
│   └── ddd-workflow/        # DDD 工作流技能
│       ├── SKILL.md         # 技能入口（触发条件 + 工作流）
│       └── references/
│           ├── initialization.md  # 项目初始化步骤
│           └── templates.md       # 文档模板
└── README.md
```

## License

MIT
