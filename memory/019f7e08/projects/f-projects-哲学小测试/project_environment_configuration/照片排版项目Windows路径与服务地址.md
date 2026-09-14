---
title: "照片排版项目Windows路径与服务地址"
usage_scenario:
    - "访问前端HTML页面时定位文件路径"
    - "启动或调试后端服务时确认监听地址"
    - "部署时配置服务端口和静态资源路径"
keywords:
    - "Windows路径"
    - "照片排版"
    - "localhost:51888"
    - "server.js端口"
---

项目根目录位于 'f:/projects/Python/照片排版/'，源码服务位于 'f:/projects/Python/照片排版/server/server.js'，前端静态资源在 server/public/，后端服务运行在 http://localhost:51888（默认端口在 server.js 的 const PORT = process.env.PORT || 51888，build.js 打包脚本中打开浏览器地址与启动.bat 生成内容也需同步该端口）。
