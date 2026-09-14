# vibe coding 资产（Qoder 配置备份）

个人 AI 编程助手（Qoder）的核心配置备份，只含 **Qoder 本体的三类资产**：协作规则、记忆库（人格）、技能来源清单。

> 用途：换电脑 / 重装系统后恢复 AI 对你的全部认知与协作方式。

## 目录结构

| 路径 | 内容 |
|------|------|
| `rules/revolutionary-collaboration-protocol.md` | 革命性协同工作规程（AI 协作总纲，15 模块，Qoder 用户级规则） |
| `rules/preferences/long-term-preferences.md` | 长期偏好记录（P-编号条目） |
| `memory/` | Qoder 记忆库全量备份（"人格"主体，1065 个文件） |
| `memory/<用户UUID>/global/` | 全局记忆：用户偏好、经验、规范、踩坑记录等 24 类 |
| `memory/<用户UUID>/projects/` | 各项目专属记忆 |
| `EXTERNAL-SKILLS.md` | 第三方技能来源清单（**只留链接不打包**，换机按链接重装、随时拉上游更新） |

## 恢复 / 安装

1. **规则**：把 `rules/*.md` 复制到 `%USERPROFILE%\.qoder\rules\`（新会话生效）
2. **记忆**：把 `memory\<用户UUID>\` 覆盖到 `%USERPROFILE%\.qoder\memories\` 下（UUID 目录名按本机实际为准）
3. **技能**：按 `EXTERNAL-SKILLS.md` 的来源链接重新安装

## 分享边界（重要）

- `memory/` 包含**个人工作记忆**（项目细节、路径等），**公开分享前必须剔除**；
- `rules/` 为通用协作方法论，可直接分享。

## 许可

[MIT License](LICENSE)
