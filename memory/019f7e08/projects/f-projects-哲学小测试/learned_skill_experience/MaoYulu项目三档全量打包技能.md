---
title: "MaoYulu项目三档全量打包技能"
usage_scenario:
    - "需要重新编译生成所有字体档位的APK安装包"
    - "执行完整的构建流水线以验证最新改动"
keywords:
    - "全量打包"
    - "APK构建"
    - "字体档位"
    - "build_pack"
---

## 输入
- 用户指令：执行全量打包（如“跑一遍”）

## 步骤
1. **执行打包脚本**：运行 `powershell -ExecutionPolicy Bypass -File "f:\projects\maoyulu\build_pack.ps1"`
2. **监控终端输出**：使用 `GetTerminalOutput` 检查构建状态，等待 BUILD SUCCESSFUL
3. **验证产物**：确认 `dist/` 目录下生成三个 APK 文件（large/medium/mini），并记录体积

## 输出
- 三个档位 APK 文件位于 `dist/` 目录
- 构建日志显示各档位成功完成

## 注意事项
- 脚本会自动处理语料库同步、字体子集化、逐档过滤、Gradle 构建及产物整理
- 若字符集未变化，字体子集化步骤会跳过
- 确保 JAVA_HOME 已正确配置
