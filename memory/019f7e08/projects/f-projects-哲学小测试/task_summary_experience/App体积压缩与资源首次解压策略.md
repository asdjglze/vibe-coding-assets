---
title: "App体积压缩与资源首次解压策略"
usage_scenario:
    - "Android应用包体积过大优化"
    - "大型字体库/数据库资源的懒加载方案"
    - "Assets资源打包与运行时解压处理"
keywords:
    - "体积压缩"
    - "gzip"
    - "资源懒加载"
    - "首次解压"
---

## 任务描述
在不改变功能的前提下压缩 APK 体积：资源进包压缩、首次启用解压到工作域，之后零重复解压。

## 最终实现（已落地并编译验证）
1. 压缩手段：删除 build.gradle.kts 的 noCompress="ttf"，让 APK 对字体自动 deflate（与 gzip 同压缩率 53%）。不需要 .gz 包装文件——AssetManager 读 deflate 条目时透明解压，运行时流式拷贝即可，零压缩算法代码。
2. 首次启用解压：FontExtractor（object）后台低优先级线程把 assets/fonts/*.ttf（不含 preview_）增量拷贝到 files/fonts/，按"存在且大小==assets条目长度"跳过、半文件校验删除重试、档位降级时清理档外残留。挂在 QuoteApplication.onCreate。
3. 消费方统一 files 优先 + assets 回退（解压未完成时兜底）：FontManager.fontFamily 用 Typeface.createFromFile+FontFamily(typeface)；WidgetCardRenderer.loadTypeface 用 createFromFile。
4. WebView 分享卡片：baseUrl 改虚拟域 https://appassets.androidplatform.net/assets/share_templates/{file}，ShareCardWebView 自实现 shouldInterceptRequest 拦截：fonts/ 路径→files/assets 字体（响应加 Access-Control-Allow-Origin 与 Cache-Control），其余→assets 流。share_core.js 导出收集逻辑从仅 file: 前缀扩展为也收集该虚拟域 https URL（XHR 同源读取转 data URI）。
5. 附带体积收益：删除 assets 残留 quote.db.bak（APK 内 6MB）与 share_templates/fonts 重复字体目录（10.4MB，与 assets/fonts 完全重复）；build.bat/build_pack.ps1 加 *.bak 防御清理。
6. 实测：large 档 21 款全量字体 APK 99MB→58.1MB（省约 41%）；STORE 条目归零。

## 注意事项
- 删除 share_templates/fonts 的前提是 WebView 拦截器已就绪，否则模板字体 404 回退宋体。
- 模板 @font-face 相对路径 ../../fonts/x.ttf 在虚拟域下解析为 /assets/fonts/，无需改任何模板文件。
- 首次启动解压 30.8MB→58MB 约 2-4 秒（后台线程，UI 无感）；消费方在解压完成前走 assets 回退。
