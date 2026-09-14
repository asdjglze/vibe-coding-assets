---
title: "Compose Text组件排版参数统一使用style写法"
usage_scenario:
    - "Compose Text组件行距/字号等排版参数不生效"
    - "不同渲染模式（如滚动与分页）排版效果不一致"
    - "排查Compose UI渲染异常"
keywords:
    - "Compose"
    - "Text"
    - "排版参数"
    - "style"
    - "lineHeight"
---

## 输入
- Compose Text组件排版参数（如行高、字号等）

## 步骤
1. 检查当前Text组件是否混用独立参数（如`lineHeight = ...`）和style参数
2. 将所有排版样式（fontFamily, fontSize, lineHeight, fontWeight, letterSpacing, textAlign, color, textIndent）统一放入`style = TextStyle(...)`参数中
3. 移除独立的排版参数调用

## 输出
Text组件渲染效果与预期一致（如行距生效）

## 注意事项
- 在某些Compose版本或设备上，同时使用独立参数和style可能导致部分样式（特别是lineHeight）被覆盖或不生效
- 滚动模式与翻页模式的渲染代码应保持完全一致的写法以避免差异
