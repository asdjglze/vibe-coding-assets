---
title: "PDC登录器开发与前端数字格式化及Bug修复"
usage_scenario:
    - "开发需要用户交互的自动化登录工具(扫码/滑块)"
    - "前端静态资源更新需绕过强缓存(整链重命名模式)"
    - "处理第三方API返回结构不一致(Dict/List混合)的解析逻辑"
keywords:
    - "PDC登录器"
    - "Playwright稳定性"
    - "前端缓存绕过"
    - "API响应解析"
---

## 任务描述
1. 开发PDC并线登录器：通过Playwright启动Edge浏览器弹窗，自动捕获用户扫码后的localStorage令牌并写入配置文件。
2. 修改前端首页数字格式：将"在库 X 册"改为中国习惯的万位分组（如5,0479）。

## 执行过程
```mermaid
graph TD
    A[需求: PDC登录器 & 数字格式化] --> B[环境侦查: Playwright/Edge可用性]
    B --> C[开发v1登录器: 临时会话目录+3秒轮询]
    C --> D[发现v1缺陷: 页面跳转导致驱动崩溃/会话丢失]
    D --> E[重构v3登录器: 独立浏览器进程+CDP接管+持久化Profile]
    E --> F[定位数字格式化函数: HomeView-custom1.js中的ae()]
    F --> G[尝试局部替换: 仅改HomeView引发环状引用冲突]
    G --> H[修正策略: 整链五文件(custom1->custom2)批量重命名]
    H --> I[部署至服务器并SHA校验]
    I --> J[发现判定Bug: pdc_site.search返回Dict被误判为List]
    J --> K[修复pdc_login.py与cover_select.py中的判定逻辑]
    K --> L[端到端实测验证: Token有效性与封面通道连通性]
```

## 任务总结
成功上线PDC登录器(v3防丢版)与数字格式化补丁。修复了三个关键问题：
1. **Playwright稳定性**：改用独立浏览器进程+CDP，避免自动化特征导致的页面自关或驱动崩溃。
2. **资源缓存隔离**：采用整链重命名(custom1->custom2)解决Vite打包的环状引用冲突。
3. **API响应解析**：修正了pdc_site.search返回Dict(list字段)时的类型判定错误，恢复了PDC查词与封面下载功能。
