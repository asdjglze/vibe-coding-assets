---
title: "Node桌面应用打包体积优化"
usage_scenario:
    - "Node.js桌面应用（如Electron/PKG/Nexe）打包体积过大时的诊断与优化"
    - "构建脚本需支持版本化输出、运行时依赖精准打包、历史产物自动清理的场景"
    - "排查node_modules冗余或重复二进制导致dist体积异常的问题"
keywords:
    - "打包体积"
    - "build.js优化"
    - "运行时依赖"
---

## 任务描述
优化照片排版软件的打包体积（原363MB），解决 node_modules 冗余、重复 node.exe、输出目录混杂等问题。

## 执行过程
```mermaid
graph TD
    A[问题定位] --> B[分析dist/node_modules占比199.8MB]
    B --> C[识别冗余包：lucide-static 45.8MB、@typescript 27MB等]
    C --> D[发现重复node.exe：runtime/node.exe + 根目录遗留各81.8MB]
    D --> E[设计优化方案：版本化输出目录、精准运行时依赖收集、根目录清理、ZIP分发]
    E --> F[实现collectProdDeps函数：递归解析dependencies+过滤wasm/非平台optionalDependencies]
    F --> G[修改build.js：添加BUILD_TAG生成、清理dist根目录旧文件、只复制PROD_ENTRY_PKGS依赖、集成ZIP打包]
    G --> H[进一步优化：pruneNodeModules清理md/map/d.ts/test等非运行时文件675项]
    H --> I[UPX -9压缩node.exe：81.8MB→25.9MB]
    I --> J[最终体积：目录58.6MB，ZIP 38MB]
```

## 任务总结
成功将打包体积从363MB缩减至58.6MB（ZIP 38MB）：通过重构build.js，实现版本化输出目录（如照片排版-v1.0.0-20240810-1530/）、仅打包server.js实际依赖的9个运行时包（剔除devDependencies及跨平台二进制）、自动清理dist根目录历史残留、pruneNodeModules删除文档测试类文件、UPX压缩便携node.exe（82→26MB，压缩后运行正常含sharp原生模块）、并生成ZIP分发包。

## 关键经验
- 便携 node.exe 可安全用 UPX -9 压缩，压缩后 express/sharp/pdfkit 均正常，收益约 55MB
- sharp 的 optionalDependencies 只保留当前平台已安装的非 wasm 包（@img/sharp-win32-x64）
- pruneNodeModules 必须跳过 @img 目录（原生模块内部结构不能动）
- 版本化输出目录构建前需清理 dist 根目录无版本旧目录（node_modules/public/runtime）与散落文件
