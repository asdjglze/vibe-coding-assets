---
title: "Chrome 浏览器自动化找不到可执行文件的排查路径"
usage_scenario:
    - "使用浏览器自动化工具时报找不到 Chrome 可执行文件"
    - "需要配置或验证 Chrome 安装路径以支持自动化测试"
keywords:
    - "Chrome"
    - "自动化"
    - "路径"
    - "可执行文件"
---

浏览器自动化工具（如 chrome-devtools）默认在以下路径查找 Google Chrome 可执行文件，若报错 `Could not find Google Chrome executable`，需确认 Chrome 实际安装在这些路径之一：
- C:\Program Files\Google\Chrome\Application\chrome.exe
- C:\Program Files (x86)\Google\Chrome\Application\chrome.exe
- C:\Users\win-user\AppData\Local\Google\Chrome\Application\chrome.exe
- D:\Program Files\Google\Chrome\Application\chrome.exe
- D:\Program Files (x86)\Google\Chrome\Application\chrome.exe
（来源：CallMcpTool）
