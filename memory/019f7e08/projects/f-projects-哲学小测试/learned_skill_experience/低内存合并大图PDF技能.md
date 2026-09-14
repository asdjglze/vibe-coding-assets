---
title: "低内存合并大图PDF技能"
usage_scenario:
    - "批量图片转PDF时遇到内存不足报错"
    - "需要合并数百页以上的大规模图片集"
    - "对PDF进行元数据更新但不想重新生成正文"
keywords:
    - "img2pdf"
    - "PyMuPDF"
    - "低内存"
    - "流式合并"
    - "增量保存"
---

## 输入
- 需要合并大量图片生成PDF的场景
- 图片数量多或单图尺寸大，直接加载会导致内存溢出

## 步骤
1. **安装 img2pdf**：`pip install img2pdf`
2. **流式合并正文**：使用 `img2pdf.convert(image_files, outputstream=file)` 将图片逐页写入PDF，内存峰值仅相当于单页大小。
3. **增量补写元数据**：使用 PyMuPDF (fitz) 打开生成的PDF，调用 `doc.set_metadata()` 设置标题/作者等，最后以 `doc.save(path, incremental=True, encryption=fitz.PDF_ENCRYPT_KEEP)` 模式保存，避免重建整个文档结构。

## 输出
- 成功生成包含正确元数据的PDF文件
- 处理过程中内存占用保持低位（不随页数线性增长）

## 注意事项
- `img2pdf` 默认按文件名排序，需确保文件名有序（如 `0001.jpg`）。
- `incremental=True` 模式不会改变现有页面内容，仅追加尾段，适合修补元数据。
- 如果不需要加密，可省略 `encryption` 参数；若需加密且保留增量特性，必须指定 `fitz.PDF_ENCRYPT_KEEP`。
