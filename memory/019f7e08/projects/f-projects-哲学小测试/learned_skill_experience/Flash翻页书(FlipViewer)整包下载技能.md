---
title: "Flash翻页书(FlipViewer)整包下载技能"
usage_scenario:
    - "网站提供的是Flash(SWF)格式的电子书，无法直接下载JPG/PDF"
    - "需要离线保存网页版电子书的所有原始素材"
keywords:
    - "Flash"
    - "SWF"
    - "FlipViewer"
    - "整包下载"
    - "XML解析"
---

## 输入
- 电子书阅读页 URL（通常是 FlipViewerXpress 系统生成的 HTML）

## 步骤
1. 请求页面 HTML，提取 `<title>` 作为书名
2. 在 HTML 中搜索 `urlOfFlipBook` 或 `.xml` 配置文件的引用路径（注意文件名可能包含空格）
3. 根据相对路径定位 XML 配置文件并读取内容
4. 从 XML 中提取所有 SWF 页面文件、封面图、文本数据等资源的绝对 URL
5. 按原目录结构批量下载这些资源到本地文件夹

## 输出
一个包含该书所有原始文件（index.html, xml, swf, jpg等）的完整离线目录

## 注意事项
- Flash 翻页书（FlipViewerXpress）通常不提供直接的 JPG 图片，而是通过 SWF 播放
- XML 配置文件中引用的资源路径可能是相对路径，需结合基础 URL 拼接
