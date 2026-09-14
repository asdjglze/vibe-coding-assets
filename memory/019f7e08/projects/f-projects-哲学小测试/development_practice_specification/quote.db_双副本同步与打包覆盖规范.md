---
title: "quote.db 双副本同步与打包覆盖规范"
usage_scenario:
    - "修改语录数据库数据时"
    - "打包前检查数据库一致性时"
    - "排查数据修改被覆盖丢失时"
keywords:
    - "quote.db"
    - "双副本"
    - "notes_editor"
    - "assets"
    - "打包覆盖"
---

maoyulu 项目的 quote.db 存在双副本：notes_editor\quote.db 是主数据源（校对工具使用），app\src\main\assets\databases\quote.db 是打包副本。build.bat 与 build_pack.ps1 打包时无条件用 notes_editor 副本覆盖 assets 副本，因此任何数据库数据修改必须两个库同时执行（或只改 notes_editor 主库后重新拷贝），只改 assets 副本会在下次打包时被覆盖丢失。修改后需重算 app\src\main\assets\databases\quote.db.md5（32 字符 MD5，DatabaseUpdater 用它判断版本更新）。用户数据严禁写入 quote.db：阅读统计存独立 stats.db（ReadStatsRepository），文本收藏书签存独立 bookmarks.db（Bookmark 实体 + BookmarksDatabase + BookmarksRepository，书信息存快照，防内容库更新后收藏失效）。
