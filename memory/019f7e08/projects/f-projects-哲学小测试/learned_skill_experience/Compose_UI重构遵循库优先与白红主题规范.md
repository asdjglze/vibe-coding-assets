---
title: "Compose UI重构遵循库优先与白红主题规范"
usage_scenario:
    - "重构Compose翻页或列表组件时"
    - "统一应用主题色或修复UI不一致问题时"
keywords:
    - "VerticalPager"
    - "Compose重构"
    - "朱砂红"
    - "主题色统一"
---

## 输入
- 需要重构的UI组件（如翻页模式、主题色等）

## 步骤
1. **主题色统一**：检查全局按钮/开关颜色，将非预期的黑色（如MoHei）统一替换为项目主色调朱砂红（ZhuSha），确保主页与设置页视觉一致。
2. **翻页模式重构**：
   - 删除自绘的分页容器（如大Column+offset平移）。
   - 引入官方 `androidx.compose.foundation.pager.VerticalPager` 及 `rememberPagerState`。
   - 保留原有的行级测量与贪心装箱分页逻辑（packArticle等）。
   - 将页码状态交由Pager管理，外部通过scrollToPage程序化跳转，内部通过currentPage回传。
3. **功能对齐**：确保重构后的翻页模式支持原有交互（如长按选区、分享入口、搜索高亮）。

## 输出
- 编译通过的代码，UI表现符合设计规范（白底朱砂红）。
- 翻页手势流畅，无自定义布局导致的性能或交互问题。

## 注意事项
- **禁止重复造轮子**：翻页等通用交互必须使用Compose Foundation库（如VerticalPager），严禁自行实现手势处理或布局偏移。
- **主题一致性**：项目中“翻页模式”不应影响主页主题色，需解绑inkMode与pagingMode的绑定关系。
