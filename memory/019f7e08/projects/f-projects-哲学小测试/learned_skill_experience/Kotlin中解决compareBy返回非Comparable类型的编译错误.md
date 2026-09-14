---
title: "Kotlin中解决compareBy返回非Comparable类型的编译错误"
usage_scenario:
    - "使用 compareBy 排序时报 Return type mismatch 错误"
    - "需要对 List 或 Map 等不可比较类型进行排序"
    - "实现复杂的自定义排序逻辑"
keywords:
    - "Kotlin"
    - "compareBy"
    - "Comparator"
    - "排序"
---

## 输入
- 待排序的列表
- 用于生成排序键的函数（可能返回复杂类型）

## 步骤
1. 尝试使用 `sortedWith(compareBy { ... })` 进行排序
2. 若编译器报错提示返回类型不匹配（如 `expected 'Comparable<*>?', actual 'List<T>'`），说明 `compareBy` 的选择器返回值必须是 `Comparable` 类型
3. 改用 `sortedWith` 配合 Lambda `(a, b) -> Int` 手写比较逻辑
4. 在 Lambda 内部实现自定义比较算法（如逐位比较数组/列表元素）

## 输出
编译通过的代码及正确的排序结果

## 注意事项
- Kotlin 标准库 `compareBy` 要求选择器返回 `Comparable?` 类型，无法直接处理 `List`、`Map` 等非 Comparable 容器作为排序依据
- 当需要按复合结构（如笔画序列）排序时，必须手动实现逐位比较逻辑
