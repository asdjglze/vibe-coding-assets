---
title: "超高卡片导出防 OOM 与视口适配技能"
usage_scenario:
    - "分享卡片内容过长导致安卓侧导出时内存溢出（OOM）"
    - "超高卡片在预览界面显示不全或滚动卡顿"
    - "需要导出高分辨率长图但受限于设备内存"
keywords:
    - "WebView 导出"
    - "Canvas 转图"
    - "OOM 修复"
    - "CSS Zoom"
    - "分块回传"
---

## 超高卡片导出防 OOM 与视口适配技能

### 输入
- 分享卡片 WebView 实例（可能包含几百行长文本导致自然高度远超屏幕）
- 需要导出高分辨率图片的需求

### 步骤
1. **视口适配（预览阶段）**：
   - 计算卡片自然高度与自然视口高度
   - 若自然高度 > 视口高度，设置 `document.documentElement.style.zoom = (视口高度 / 自然高度)`
   - 使整卡缩小填满视口，WebView 视图本身不扩高，避免渲染量随内容线性增长
2. **导出准备**：
   - 点击导出前，先重置 `document.documentElement.style.zoom = ''` 获取卡片真实自然宽高
3. **Web 侧 Canvas 转图**：
   - 克隆卡片 DOM，聚合页面全部样式（含 @font-face、伪元素规则）
   - 将相对路径 url() 转为绝对路径（防止 SVG blob 文档中资源失效）
   - 构建 SVG foreignObject 包裹卡片内容，转为 Image 对象
   - 在 canvas 上绘制 Image，生成 PNG Blob
4. **分块回传**：
   - 将 Base64 字符串按 256KB 分块
   - 通过 JS 桥逐块发送 (`onImageChunk(seq, total, chunk)`)，避免单次传输大字符串导致 JS 线程卡死
   - 原生层接收拼块，收齐后返回完整 Base64

### 输出
- 预览：卡片完整显示在视口内，无滚动条，渲染流畅
- 导出：高分辨率图片文件（自然尺寸，不受缩放影响），无 OOM 崩溃

### 注意事项
- **禁止安卓侧 view.draw**：超高卡片按物理像素建 Bitmap 会直接 OOM（几百行 → 数百 MB），必须走 Web 侧 canvas
- **Zoom 处理**：导出测量前必须重置 zoom，否则 rect 获取的是缩放后尺寸；导出完成后恢复 zoom 保证预览正常
- **分块大小**：建议 256KB，过大易卡死 JS 桥，过小增加通信开销
- **字体加载**：确保 `document.fonts.ready` 后再执行快照，避免使用回退字体
