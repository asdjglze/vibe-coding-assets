# 外部技能来源清单

本仓库遵循原则：**别人的技能只记录来源，不打包文件**——换设备时按下方链接重新安装，并可随时拉取上游更新。

## 第三方技能一览

| 本地技能名 | 上游来源 | 说明 |
|-----------|---------|------|
| brainstorming / systematic-debugging / test-driven-development / writing-plans / executing-plans / requesting-code-review / receiving-code-review / subagent-driven-development / dispatching-parallel-agents / using-git-worktrees / finishing-a-development-branch / verification-before-completion / using-superpowers / writing-skills | <https://github.com/obra/superpowers> | 软件开发方法论技能集（头脑风暴、系统化调试、TDD、计划、代码审查等） |
| planning-with-files-skill | <https://github.com/othmanadi/planning-with-files> | 基于文件的持久化任务规划（task_plan / findings / progress 三文件法） |
| ui-ux-pro-max | <https://github.com/nextlevelbuilder/ui-ux-pro-max-skill> | UI/UX 设计智能（含风格/配色/字体数据与查询脚本） |
| claude-design | <https://github.com/freshtechbro/claudedesignskills> | Three.js / WebGL / 3D Web 开发技能（Claude Design Skillstack 之一） |
| gsap-animation | <https://github.com/greensock/gsap-skills> | GSAP 官方 AI 技能 |
| motion-anything | <https://github.com/nexu-io/motion-anything> | 动效引擎（chat-native motion layer） |
| motion-design | <https://github.com/iart-ai/motion-design-skills> | 动效设计（iart.ai 15 个技能包之一，另有 kinetic-typography / webgl-animation 等 14 包同源） |
| web-animation-design | <https://github.com/iart-ai/web-animation-skills> | Web 动画设计（基于 Emil Kowalski《Animations on the Web》课程） |

## 安装方式（Qoder 本地插件）

1. 建立插件目录，例如 `%USERPROFILE%\.qoder\plugins\cache\local\superpowers\`
2. 放入 `plugin.json`（关键字段：`"skills": "./skills/"`、`"marketplaceName": "local"`）
3. 把各技能目录（每个含 `SKILL.md`）放进 `skills\` 子目录
4. 重启 Qoder，在设置 → 插件中确认启用

## 更新方式

定期访问上表链接，拉取上游最新版本，覆盖本地 `skills\` 对应目录即可。
