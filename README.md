# DOCX MCP 服务

一个功能强大的 Word 文档处理 MCP 服务，提供文档结构提取、内容修改、云存储集成等完整的文档处理解决方案。支持从URL下载文档、批量修改内容、自动上传到阿里云OSS等功能，完全兼容MCP协议，可无缝集成到各种AI助手中。

## 🚀 服务功能

- **📄 文档结构提取**: 智能解析 .docx 文件，提取段落、表格等结构化内容，为每个元素分配唯一ID
- **✏️ 批量内容修改**: 支持基于元素ID的精确文本替换和内容更新
- **☁️ 云存储集成**: 自动上传修改后的文档到阿里云OSS，提供便捷的下载链接
- **🔗 URL文档处理**: 直接从网络URL下载、处理并重新上传文档
- **🛠️ MCP标准兼容**: 完全符合Model Context Protocol规范，支持标准MCP客户端

## 📋 系统要求

- Python 3.13+
- uvx (Python包管理器，推荐)

## 🔧 服务配置

### Server config

```json
{
  "command": "uvx",
  "args": ["docx-mcp"]
}
```

## 🛠️ 可用工具

### 1. extract_document_structure
从URL下载并解析.docx文件的结构

**参数:**
- `document_url` (string): .docx文件的URL链接

**返回:** 包含文档结构的字典，每个元素都有唯一ID

### 2. apply_modifications_to_document  
将修改应用到.docx文件

**参数:**
- `original_file_content_base64` (string): 原始文件的Base64编码
- `patches_json` (string): JSON格式的修改指令列表

**返回:** 修改后文件的Base64编码

### 3. get_modified_document
获取修改后的文档（apply_modifications_to_document的别名）

**参数:** 同apply_modifications_to_document

### 4. prepare_document_for_download
将修改后的文档上传到阿里云OSS

**参数:**
- `original_file_content_base64` (string): 原始文件的Base64编码  
- `patches_json` (string): JSON格式的修改指令

**返回:** 包含上传结果和下载链接的字典

### 5. process_document_from_url
从URL下载文档，应用修改，上传到OSS的完整流程

**参数:**
- `document_url` (string): 原始文档的URL
- `patches_json` (string): JSON格式的修改指令

**返回:** 包含处理结果和下载链接的字典

## 📝 使用示例

### 修改指令格式

```json
[
  {
    "element_id": "p_0",
    "new_content": "新的段落内容"
  },
  {
    "element_id": "table_0_cell_0_0", 
    "new_content": "新的表格单元格内容"
  }
]
```

### 典型工作流程

1. **提取文档结构**: 使用`extract_document_structure`获取文档中所有元素的ID
2. **准备修改指令**: 根据元素ID创建修改指令JSON
3. **处理文档**: 使用`process_document_from_url`一键完成下载、修改、上传
4. **获取结果**: 从返回的下载链接获取处理后的文档

## 🚀 快速开始

### 通过uvx运行（推荐）

```bash
# 直接运行（已发布到PyPI）
uvx docx-mcp

# 从本地项目运行
uvx --from . docx-mcp
```

### 配置到MCP客户端

在支持MCP的客户端中添加以下配置：

```json
{
  "mcpServers": {
       "docx_filler_service": {
      "command": "cmd",
      "args": [
        "/c",
        "uvx",
        "docx-mcp"
      ]
    }
  }
}
```

## ✨ 部署优势

- **🚀 零配置部署**: 无需配置环境变量，开箱即用
- **📦 一键安装**: 通过uvx直接运行，自动处理依赖
- **🔒 内置配置**: OSS存储配置已内置，简化部署流程
- **⚡ 即时可用**: 安装后立即可以处理文档

## 📁 项目结构

```
docx_mcp/
├── core/                    # 核心功能模块
│   ├── docx_processor.py   # 文档处理器
│   └── models.py           # 数据模型定义
├── main.py                 # MCP服务主入口
├── pyproject.toml         # 项目配置和依赖定义
├── requirements.txt       # 依赖列表
├── LICENSE               # MIT许可证
└── README.md             # 项目文档
```

## 🔧 技术栈

- **MCP框架**: FastMCP - 高性能MCP服务框架
- **文档处理**: python-docx - Office文档操作库
- **云存储**: 阿里云OSS Python SDK
- **包管理**: uvx/uv - 现代Python包管理工具

## 📊 性能特性

- **内存高效**: 流式处理大文档，避免内存溢出
- **并发安全**: 支持多个客户端同时访问
- **错误恢复**: 完善的异常处理和错误恢复机制
- **格式兼容**: 支持Office 2007+的.docx格式
- **零配置**: 内置云存储配置，无需额外设置

## 🤝 贡献指南

1. Fork本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启Pull Request

## 📄 许可证

本项目基于MIT许可证开源 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🔗 相关链接

- [MCP官方文档](https://modelcontextprotocol.io/)
- [FastMCP框架](https://github.com/pydantic/fastmcp)
- [python-docx文档](https://python-docx.readthedocs.io/)
- [阿里云OSS Python SDK](https://help.aliyun.com/document_detail/32026.html)

## ❓ 常见问题

**Q: 需要配置什么环境变量吗？**
A: 不需要！OSS配置已内置到服务中，开箱即用。

**Q: 支持哪些文档格式？**
A: 目前仅支持.docx格式（Office 2007+格式）。

**Q: 文档大小限制？**
A: 建议单个文档不超过50MB，以确保最佳性能。

**Q: 如何开始使用？**
A: 只需运行 `uvx docx-mcp` 即可启动服务，无需任何配置。

**Q: 文档会存储在哪里？**
A: 处理后的文档会自动上传到预配置的阿里云OSS存储，并提供下载链接。

---

💡 **提示**: 如有问题或建议，欢迎提交Issue或Pull Request！
