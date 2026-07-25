# wcpe — Plugin Marketplaces

WCPE 的插件市场索引仓库，当前仓库名为 `plugin-marketplaces`。本仓库同时维护 **Claude Code** 与 **OpenAI Codex** 两类插件市场索引；插件源码仍保留在各自独立仓库中，本仓库只负责声明插件条目和源码仓库地址。

## 市场索引结构

```text
plugin-marketplaces/
├── .claude-plugin/
│   └── marketplace.json        # Claude Code 插件市场
├── .agents/
│   └── plugins/
│       └── marketplace.json    # OpenAI Codex 插件市场
└── README.md
```

## Claude Code 用法

```bash
/plugin marketplace add wcpe/plugin-marketplaces      # 加这个市场（一次；wcpe/plugin-marketplaces = 本仓库的 github owner/repo）
/plugin install sdd-skills@wcpe                       # 装 SDD 规格驱动开发技能集
/plugin install mc-testkit@wcpe                       # 装 mc-testkit E2E / serve 对接
/plugin install privacy-guard@wcpe                    # 装隐私/敏感数据检测工具
/plugin install security-scan@wcpe                    # 装分层代码安全扫描与修复闭环
```

## Codex 用法

```bash
codex plugin marketplace add D:\ProjectsSkill\plugin-marketplaces
```

添加市场后，在 Codex 插件界面安装 `sdd-skills@wcpe`、`mc-testkit-skill@wcpe`、`privacy-guard@wcpe` 或 `security-scan@wcpe`。

## Claude Code 收录的插件

| 插件 | 仓库 | 内容 |
|---|---|---|
| `sdd-skills` | [wcpe/sdd-skills](https://github.com/wcpe/sdd-skills) | 18 个 SDD 技能（2 脚手架 + 16 迭代工作流），纯技能包 |
| `mc-testkit` | [wcpe/mc-testkit-skill](https://github.com/wcpe/mc-testkit-skill) | mc-testkit E2E 编排 + serve 持久手测：1 技能 + 3 命令 + 护栏 hook + MCP |
| `privacy-guard` | [wcpe/privacy-guard-skill](https://github.com/wcpe/privacy-guard-skill) | 隐私/敏感数据检测：工作区与 git 历史扫描、脱敏分级报告、提交门护栏 hook + MCP |
| `security-scan` | [wcpe/security-scan](https://github.com/wcpe/security-scan) | 分层代码安全扫描与修复闭环：L1/L2/L3/全量四模式，规则预筛 + 模型验证污点链路，产出带证据的 findings 与 HTML 报告，确认后修复并复扫 |

## Codex 收录的插件

| 插件 | 源码仓库 | 内容 |
|---|---|---|
| `sdd-skills` | [wcpe/sdd-skills](https://github.com/wcpe/sdd-skills) | SDD 规格驱动开发技能集：18 个 workflow skills、Codex 展示元数据、workflow skill cards、MCP 漂移审计工具。 |
| `mc-testkit-skill` | [wcpe/mc-testkit-skill](https://github.com/wcpe/mc-testkit-skill) | mc-testkit E2E 编排与 serve 持久手测：技能、只读 MCP、Codex 展示元数据、内置模板资产。 |
| `privacy-guard` | [wcpe/privacy-guard-skill](https://github.com/wcpe/privacy-guard-skill) | 隐私/敏感数据检测：工作区与 git 历史扫描、脱敏分级报告、修复指引。 |
| `security-scan` | [wcpe/security-scan](https://github.com/wcpe/security-scan) | 分层代码安全扫描与修复闭环：L1/L2/L3/全量四模式，规则预筛 + 模型验证污点链路，产出带证据的 findings 与 HTML 报告，确认后修复并复扫。 |

## 加新 Claude Code 插件

往 `.claude-plugin/marketplace.json` 的 `plugins` 数组加一项，`source` 用**显式 HTTPS**：

```json
{ "source": "url", "url": "https://github.com/wcpe/<repo>.git" }
```

插件代码不进本仓库。

## 加新 Codex 插件

在 `.agents/plugins/marketplace.json` 的 `plugins` 数组中添加对应条目。插件在 GitHub 仓库根目录时使用：

```json
{
  "name": "plugin-name",
  "source": {
    "source": "url",
    "url": "https://github.com/wcpe/plugin-name.git"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Productivity"
}
```

## Claude Code 团队自动分发

在**消费方仓库**的 `.claude/settings.json` 里声明，clone 的人自动装上：

```jsonc
{
  "extraKnownMarketplaces": {
    "wcpe": { "source": { "source": "github", "repo": "wcpe/plugin-marketplaces" } }
  },
  "enabledPlugins": {
    "sdd-skills@wcpe": true,
    "mc-testkit@wcpe": true,
    "privacy-guard@wcpe": true,
    "security-scan@wcpe": true
  }
}
```

> 跨仓库插件的 `source` 用 `{ "source": "url", "url": "https://….git" }`（显式 HTTPS，已对照可用市场 superpowers-marketplace 验证）。**别用** `{ "source": "github", "repo": "…" }`——该形式会走 SSH 克隆（`git@github.com:`），没配 GitHub SSH 主机密钥 / 账号密钥的机器会报 `Host key verification failed`。
