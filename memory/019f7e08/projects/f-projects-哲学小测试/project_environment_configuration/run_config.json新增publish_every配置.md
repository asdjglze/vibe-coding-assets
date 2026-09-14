---
title: "run_config.json新增publish_every配置"
usage_scenario:
    - "调整图书归档发布频率时修改该配置"
    - "排查读者端新书更新延迟问题时检查此配置"
keywords:
    - "publish_every"
    - "批内发布"
    - "配置参数"
---

项目运行配置文件 `run_config.json` 新增 `publish_every` 参数（默认值5），用于控制归档批内发布频率，每处理指定数量的书籍后触发一次增量同步，避免读者长时间等待新书可见
