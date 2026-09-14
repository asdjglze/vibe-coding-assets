---
title: "Compose 状态丢失修复：提升状态层级技能"
usage_scenario:
    - "页面切换后滚动位置重置"
    - "进出阅读页后列表回到顶部"
    - "切 Tab 后状态异常丢失"
keywords:
    - "Compose"
    - "状态丢失"
    - "层级提升"
    - "字节码分析"
    - "时序问题"
---

## 输入
- 页面切换或进出子树后，`rememberSaveable` 状态丢失（如滚动位置重置）
- 怀疑是 `SaveableStateProvider` 保存时序问题

## 步骤
1. **验证时序**：反编译 Compose 运行时 JAR（`.aar` → `.zip` → `classes.jar`），检查 `SaveableStateHolderImpl` 和 `Composer` 相关类的字节码。
2. **定位根因**：确认 `SaveableStateProvider` 在 `onDispose` 中调用 `performSave()`，但 Compose 移除子树时遵循“先子后父”原则，导致子节点注销后父节点保存时注册表已空。
3. **实施修复**：将状态提升到**永不离开组合的层级**（如 Activity 顶层、Tab 容器外层），确保状态对象在子树销毁期间依然存活。
4. **传递状态**：父级持有 `LazyGridState` 等状态对象，作为参数传递给子组件，替代子组件内部的 `rememberSaveable`。

## 输出
- 页面返回后滚动位置/状态原样保留
- 不再依赖不可靠的 `SaveableStateProvider` 离开/进入时序

## 注意事项
- 此方法适用于所有因“子树销毁重建”导致的状态丢失场景
- 需确保提升后的层级在导航/页面切换过程中始终处于组合树中
