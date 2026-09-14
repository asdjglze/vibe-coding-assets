---
title: "Node服务打包模式判断应使用路径比对而非文件后缀"
usage_scenario:
    - "Express服务返回Cannot GET且确认端口监听正常"
    - "源码运行时静态资源404但打包版正常"
    - "调试Node服务静态目录配置错误时发现isPkg判断逻辑异常"
keywords:
    - "打包模式判断"
    - "process.execPath"
    - "__dirname"
    - "Express静态资源"
---

server.js 中打包模式判断 `process.execPath.endsWith('.exe')` 存在严重缺陷：系统自带 node.exe 也以 .exe 结尾，导致源码运行时被误判为打包模式，静态目录指向错误路径（如 C:\Program Files\nodejs\public），从而返回 Cannot GET。正确判断逻辑应为 `path.dirname(process.execPath) === __dirname`，即检查 node 可执行文件是否与 server.js 在同一目录——这才是打包版的真实特征（来源：Bash + Read）
