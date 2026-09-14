---
title: "PDC 自动登录器开发与数字格式修正"
usage_scenario:
    - "第三方服务 Token 失效需人工干预时，构建自动化登录工具（弹窗+抓包+验证）"
    - "前端数字展示需适配特定地区千分位/万位分组习惯时的函数定位与修改"
keywords:
    - "PDC 登录"
    - "Playwright"
    - "Token 自动获取"
    - "数字格式化"
    - "万位分组"
---

## 任务描述
解决 PDC 令牌失效问题并实现自动登录恢复机制；同时修正前端"在库册数"显示格式为中国式万位分组（如 5,0479）。

## 执行过程
```mermaid
graph TD
    A[需求:PDC 自动登录 + 数字格式修正] --> B[环境侦查：检查 Playwright/Selenium/Edge 可用性]
    B --> C[Playwright Edge 冒烟测试通过]
    C --> D[编写 pdc_login.py：弹窗浏览器→监听 localStorage→捕获 token→现场验证→写入 state/pdc_token.txt]
    D --> E[启动后台登录器等待用户扫码登录]
    F[需求:数字格式修正] --> G[探针定位 HomeView-custom1.js 中 ae() 函数]
    G --> H[确认 ae() 仅用于"在库"册数显示]
    H --> I[修改 ae() 实现：Number(t).toLocaleString("zh-CN") 改为自定义万位分组逻辑]
    I --> J[校验本地与服务器文件 SHA256 哈希一致]
```

## 任务总结
1. **PDC 自动登录器**：新建 `pdc_login.py`，利用 Playwright 控制 Edge 弹出窗口，实时扫描 `localStorage['user-token']`，捕获后调用 `pdc_site.search()` 验证有效性，验证通过即写入 `state/pdc_token.txt` 并清除失效快照，实现零重启恢复。
2. **数字格式修正**：定位 `HomeView-custom1.js` 中的 `ae()` 函数，将其从默认的 `toLocaleString("zh-CN")`（西方三位分组）修改为支持中国习惯的万位逗号分隔（5,0479），确保本地与服务器哈希一致后生效。
