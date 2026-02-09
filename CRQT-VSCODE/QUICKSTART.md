# 🚀 CRQT VS Code 扩展 - 快速开始

## 项目已完成！ ✅

您的CRQT语言VS Code扩展项目已经完全创建完毕！

## 文件清单（15个文件）

### 📁 核心文件 (4个)
- `package.json` - 扩展配置文件
- `extension.js` - 主扩展代码
- `language-configuration.json` - 语言配置
- `.vscodeignore` - 打包忽略文件

### 📚 文档 (5个)
- `README.md` - 用户指南
- `GUIDE.md` - 快速入门指南
- `PROJECT_SUMMARY.md` - 项目总结
- `CHANGELOG.md` - 版本历史
- `QUICKSTART.md` - 本文件

### 💻 开发文件 (2个)
- `.vscode/launch.json` - VS Code调试配置
- `.gitignore` - Git配置

### 🎨 主题和语法 (2个)
- `syntaxes/crqt.tmLanguage.json` - TextMate语法定义
- `themes/crqt-default-light.json` - 颜色主题

### 📝 示例文件 (3个)
- `example.crqt` - 基础示例
- `example-detailed.crqt` - 详细示例
- `example-complex.crqt` - 复杂示例

## 立即测试

### 步骤 1: 打开项目
```powershell
cd E:\CRQT-VSCODE
code .
```

### 步骤 2: 启动开发模式
- 按 **F5** 或
- 使用 Run → Start Debugging

### 步骤 3: 查看效果
在新打开的VS Code窗口中：
- 打开 `example.crqt` 文件
- 您应该看到彩色的语法高亮！

## 标签高亮颜色

当您打开.crqt文件时，您会看到：

| 标签类型 | 颜色 | 示例 |
|---------|------|------|
| 结构标签 | 🔵 蓝色 | `<qt>`, `<tex>`, `<as>` |
| 样式标签 | 🟣 紫色 | `<str>`, `<ilt>`, `<udl>` |
| 状态标签 | 🟠 橙色 | `<fs>`, `<ft>` |
| 组件标签 | 🩷 粉色 | `<p>`, `<hx>`, `<cd#1>` |
| 字符标签 | 🔴 红色 | `<uni>` |

## 编辑说明

### 自定义颜色
编辑文件：`themes/crqt-default-light.json`

```json
{
  "scope": "entity.name.tag.paired.crqt",
  "settings": {
    "foreground": "#0066CC"  // 改变这个颜色值
  }
}
```

### 添加新标签
编辑文件：`syntaxes/crqt.tmLanguage.json`

在 `patterns` 中添加新的匹配规则。

## 支持的所有标签

**结构**：`<qt>` `<tex>` `<as>` `<opt>` `<jx>`
**样式**：`<str>` `<ilt>` `<udl>`
**状态**：`<fs>` `<ft>`
**组件**：`<p>` `<hx>` `<cd#1>` `<pt>`
**字符**：`<uni>`

## 打包扩展（可选）

当您满意时，可以将其打包为 `.vsix` 文件：

```powershell
npm install -g vsce
vsce package
```

这会创建 `crqt-support-0.0.1.vsix` 文件。

## 更多信息

- 📖 查看 `README.md` - 完整用户指南
- 📘 查看 `GUIDE.md` - 详细开发指南
- 📊 查看 `PROJECT_SUMMARY.md` - 项目技术细节

## 遇到问题？

1. **高亮不显示** - 确保是 `.crqt` 后缀的文件
2. **颜色不对** - 检查 `themes/crqt-default-light.json`
3. **标签不识别** - 检查 `syntaxes/crqt.tmLanguage.json`

## 项目结构

```
E:\CRQT-VSCODE
├── .vscode/
│   └── launch.json
├── syntaxes/
│   └── crqt.tmLanguage.json
├── themes/
│   └── crqt-default-light.json
├── example.crqt
├── example-detailed.crqt
├── example-complex.crqt
├── extension.js
├── package.json
├── language-configuration.json
├── README.md
├── GUIDE.md
├── PROJECT_SUMMARY.md
├── CHANGELOG.md
├── .gitignore
└── .vscodeignore
```

## 下一步

1. ✅ 按 F5 启动扩展
2. ✅ 打开示例文件查看效果
3. ✅ 根据需要自定义颜色和行为
4. ✅ 添加更多功能（可选）
5. ✅ 发布到VS Code市场（可选）

---

**祝您使用愉快！** 🎉

如有任何问题，请参考项目文档。
