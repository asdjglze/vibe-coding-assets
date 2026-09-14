---
title: "Windows 下配置 Android 编译 JAVA_HOME 技能"
usage_scenario:
    - "Android 项目编译时报错找不到 java 命令或 JAVA_HOME 未设置"
    - "在 Windows PowerShell 环境下进行 Gradle 构建遇到 Java 环境缺失问题"
keywords:
    - "JAVA_HOME"
    - "Android Studio"
    - "Gradle 编译"
    - "PowerShell"
---

## 输入
- Android 项目编译报错 `JAVA_HOME is not set` 或找不到 java 命令
- 工作目录为 Windows PowerShell 环境

## 步骤
1. 检查常见 JDK/JBR 路径：`C:\Program Files\Android\Android Studio\jbr` (Studio 自带)、`$env:LOCALAPPDATA\Programs\Android Studio\jbr`、`C:\Program Files\Java`、`$env:USERPROFILE\.jdks`
2. 使用 PowerShell 遍历路径确认存在：`foreach ($p in $paths) { if (Test-Path $p) { Write-Output "FOUND: $p" } }`
3. 临时设置环境变量：`$env:JAVA_HOME="<找到的 jbr 路径>"`
4. 重新执行 Gradle 编译命令

## 输出
Gradle 编译成功 (`BUILD SUCCESSFUL`)

## 注意事项
- Android Studio 内置的 JBR (JetBrains Runtime) 通常位于 `C:\Program Files\Android\Android Studio\jbr`，无需单独安装 JDK 即可编译
- PowerShell 中设置 `$env:JAVA_HOME` 仅对当前会话有效，适合临时构建调试
