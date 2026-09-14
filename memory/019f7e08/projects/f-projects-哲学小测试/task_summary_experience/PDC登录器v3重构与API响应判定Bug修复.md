---
title: "PDC登录器v3重构与API响应判定Bug修复"
usage_scenario:
    - "自动化脚本因目标网站反爬或跳转导致崩溃时的架构调整"
    - "修复因 API 返回结构变更（如 List 变 Dict）导致的业务逻辑错误"
    - "Web 自动化工具的令牌持久化与防丢设计"
keywords:
    - "PDC登录器"
    - "CDP接管"
    - "API响应判定"
    - "Playwright崩溃"
---

## 任务描述
重构 PDC 登录器以解决 playwright 崩溃导致的令牌丢失问题，并修复因 API 响应类型误判导致的验证失败及 PDC 通道失效。

## 执行过程
```mermaid
graph TD
    A[需求:解决令牌丢失与程序卡死] --> B[分析旧版 Playwright 托管模式缺陷]
    B --> C[设计 v3: 独立 Edge + CDP 端口接管]
    C --> D[实现会话落盘与断线重连机制]
    D --> E[捕获 TargetClosedError 并排查站点反爬特征]
    E --> F[发现 pdc_site.search 返回 Dict 而非 List]
    F --> G[修复 pdc_login.py 与 cover_select.py 的判定逻辑]
    G --> H[端到端实测验证 PDC 检索与封面通道]
```

## 任务总结
1. **架构升级**：放弃 Playwright 托管，改用独立浏览器进程 + CDP (9223) 采集，确保工具崩溃不影响浏览器，且支持从磁盘 Local Storage 打捞令牌。
2. **Bug 修复**：识别出 `pdc_site.search` 成功时返回 `dict`（含 `list` 字段），原代码按 `list` 判定导致永远认不出成功。已同步修复登录器验证逻辑与封面判断通道的调用逻辑。
3. **结果**：v3 版本稳定运行，新令牌一次验证通过，PDC 封面通道恢复正常。
