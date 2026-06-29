# wcpe — Claude Code 插件市场

WCPE 的 Claude Code **插件市场（marketplace）**。本仓库**不含插件代码**，只用一份 `.claude-plugin/marketplace.json` **列出** WCPE 的各插件、指向它们各自的仓库——插件代码留在它们自己的仓库，本市场只做索引。

## 用法

```bash
/plugin marketplace add wcpe/claude-plugins      # 加这个市场（一次；wcpe/claude-plugins = 本仓库的 github owner/repo）
/plugin install sdd-skills@wcpe                  # 装 SDD 规格驱动开发技能集
/plugin install mc-testkit@wcpe                  # 装 mc-testkit E2E / serve 对接
```

## 收录的插件

| 插件 | 仓库 | 内容 |
|---|---|---|
| `sdd-skills` | [wcpe/sdd-skills](https://github.com/wcpe/sdd-skills) | 18 个 SDD 技能（2 脚手架 + 16 迭代工作流），纯技能包 |
| `mc-testkit` | [wcpe/mc-testkit-skill](https://github.com/wcpe/mc-testkit-skill) | mc-testkit E2E 编排 + serve 持久手测：1 技能 + 3 命令 + 护栏 hook + MCP |

## 加新插件

往 `.claude-plugin/marketplace.json` 的 `plugins` 数组加一项，`source` 指向插件仓库（`{ "source": "github", "repo": "wcpe/<repo>" }`）即可；插件代码不进本仓库。

## 团队自动分发

在**消费方仓库**的 `.claude/settings.json` 里声明，clone 的人自动装上：

```jsonc
{
  "extraKnownMarketplaces": {
    "wcpe": { "source": { "source": "github", "repo": "wcpe/claude-plugins" } }
  },
  "enabledPlugins": {
    "sdd-skills@wcpe": true,
    "mc-testkit@wcpe": true
  }
}
```

> 字段名 / `source` 写法以你当前 Claude Code 的 `/plugin` 文档为准（插件机制在演进）。若跨仓库 `source` 对象式不被识别，备选：把插件作 git 子模块放进本仓库、`source` 改用相对路径（`"./sdd-skills"` / `"./mc-testkit"`）。
