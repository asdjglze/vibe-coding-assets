---
title: "Android 项目 UI 文本内容重构与验证技能"
usage_scenario:
    - "需要修改 Android App 界面显示的文案或段落内容"
    - "用户要求修改部分文本但需保留其他已做的微调（如标点）"
    - "修改代码后需要快速验证编译是否通过"
keywords:
    - "UI 文本修改"
    - "SearchReplace"
    - "Gradle 编译"
    - "保留修改"
---

## 输入
- 需要修改 Android 项目 UI 文本的具体内容（如重写段落、插入新观点）
- 需保留用户之前对代码的其他手动修改（如标点符号调整）

## 步骤
1. 使用 `SearchReplace` 工具定位目标文件中的对应文本块
2. 根据用户指令重写或插入新文本，注意保持原有格式（字体、字号、行高、对齐方式等）
3. 仔细检查 diff，确保未覆盖用户之前已做的非本次指令范围内的修改（如标点修正）
4. 执行编译命令（如 `gradlew :app:compileDebugKotlin`）验证语法正确性
5. 确认构建成功后交付结果

## 输出
UI 文本按新逻辑更新，且保留其他历史修改，编译构建成功（BUILD SUCCESSFUL）

## 注意事项
- 修改文本时需完整复制原有的样式参数（fontFamily, fontSize, lineHeight, textAlign, color），避免破坏布局
- 若用户提及过“保留某处修改”，在执行 SearchReplace 前需先确认当前文件状态，防止覆盖
