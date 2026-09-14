---
title: "PyMuPDF设置PDF标准元数据技能"
usage_scenario:
    - "需要为生成的PDF添加标题、作者、来源等元数据以便检索归档"
    - "PDF元数据写入后验证显示为空，排查是否为格式或文件读取问题"
    - "编写PDF生成工具时需要标准化文档属性"
keywords:
    - "PyMuPDF"
    - "PDF元数据"
    - "时间格式"
    - "fitz"
    - "文档属性"
---

## 输入
- PDF生成任务（如使用PyMuPDF/fitz）
- 需要写入的元数据信息（标题、作者、来源、关键词等）

## 步骤
1. 构建元数据字典 `meta`，包含标准字段：
   - `title`: 文档标题
   - `author`: 作者（可选）
   - `subject`: 主题/来源
   - `keywords`: 关键词列表
   - `creator`: 创建程序名称
   - `producer`: 生成库名称（如PyMuPDF）
   - `creationDate` / `modDate`: 时间戳（PDF标准格式 `D:YYYYMMDDHHMMSS+08'00'`，带时区偏移）
2. 调用 `doc.set_metadata(meta)` 写入文档对象
3. 保存文档后，用 `fitz.open().metadata` 读取验证所有字段是否生效

## 输出
生成的PDF文件在属性中可见完整的元数据信息，且时间字段正确显示

## 注意事项
- **时间格式**：必须使用 `D:YYYYMMDDHHMMSS+HH'MM'` 格式（如 `D:20260908181235+08'00'`），否则可能无法被阅读器识别
- **验证技巧**：读取元数据时需确保读取的是最新生成的文件，避免 glob 排序或缓存导致误判为“写入失败”
- **扩展资源**：PyMuPDF (fitz) 文档：https://pymupdf.readthedocs.io/
