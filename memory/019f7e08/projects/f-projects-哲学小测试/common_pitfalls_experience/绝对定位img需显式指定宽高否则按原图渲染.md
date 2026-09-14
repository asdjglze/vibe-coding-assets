---
title: "绝对定位img需显式指定宽高否则按原图渲染"
usage_scenario:
    - "分享卡片模板中头像显示过大超出容器边界"
    - "绝对定位的图片未按预期缩放而是显示原始尺寸"
keywords:
    - "img"
    - "absolute"
    - "width"
    - "height"
    - "object-fit"
---

HTML中`<img>`是替换元素，当使用`position: absolute`且未显式设置`width/height`（即`width: auto; height: auto`）时，浏览器会直接使用图片的原始像素尺寸进行渲染，此时`inset`等定位属性无法约束其大小。修复方法：必须显式给`img`设置具体的`width`和`height`值。（来源：Bash/CSS调试）
