---
title: "superpowers插件qiushi系列skill已按用户要求删除"
usage_scenario:
    - "推荐或调用skill时检查是否为已删除的qiushi系列"
    - "需要修改Qoder插件目录文件时选择正确工具"
    - "清理插件残留引用时参考本次范围"
keywords:
    - "qiushi"
    - "superpowers"
    - "skill删除"
    - "插件清理"
    - "Qoder"
---

superpowers 插件中的 qiushi-* 系列 skill（武装思想/调查研究/矛盾分析/群众路线/集中兵力/持久战略/星火燎原/统筹兼顾/实践认识论/批评与自我批评/workflows，共11个）已于2026年9月应用户要求从 C:\Users\win-user\.qoder\plugins\cache\local\superpowers\skills\ 全部删除，同时清理了 README.md 技能表和两个 plugin.json 描述中的引用。用户理由："用处不大，AI毕竟不是人类"——不建议再向用户推荐该系列或重新安装。注意：Qoder 文件编辑工具无法操作工作区外的文件，删除/修改 .qoder 插件目录需用命令行；PowerShell 命令行传中文会导致多字节字符损坏，应改用 Python 脚本（ASCII 源码 + \u 转义）。
