# CRQT VS Code Extension - 快速入门

## 项目结构

```
crqt-support/
├── .vscode/
│   └── launch.json              # 调试配置
├── syntaxes/
│   └── crqt.tmLanguage.json     # TextMate语法定义
├── themes/
│   └── crqt-default-light.json  # 颜色主题定义
├── extension.js                  # 主扩展文件
├── language-configuration.json   # 语言配置
├── package.json                  # 项目配置和元数据
├── README.md                     # 用户指南
├── CHANGELOG.md                  # 更新日志
├── example.crqt                  # 简单示例
├── example-detailed.crqt         # 详细示例
├── example-complex.crqt          # 复杂示例
└── .vscodeignore                 # 打包忽略文件
```

## 文件说明

### 核心文件

#### `package.json`
- 定义插件的元数据信息
- 指定支持的语言（CRQT）
- 注册语法定义和主题
- 配置自动关闭对和括号匹配

#### `syntaxes/crqt.tmLanguage.json`
- 使用TextMate语法定义CRQT语言
- 定义了所有标签的语法规则
- 支持嵌套和注释
- 按标签类型分类着色

#### `language-configuration.json`
- 定义语言的编辑行为
- 自动补全闭合标签
- 括号/标签匹配
- 代码折叠规则

#### `themes/crqt-default-light.json`
- 为不同标签类型定义颜色
- 结构标签：蓝色 (#0066CC)
- 样式标签：紫色 (#6600CC)
- 状态标签：橙色 (#CC6600)
- 组件标签：粉色 (#CC0066)
- 字符标签：红色 (#CC0000)

### 示例文件

#### `example.crqt`
- 基础语法示例
- 展示常用标签的使用

#### `example-detailed.crqt`
- 详细的标签用法说明
- 展示更复杂的组合

#### `example-complex.crqt`
- 嵌套标签的高级例子
- 展示多层标签组合

## 标签参考

### 结构标签（Paired - 成对）
```crqt
<qt>...</qt>      <!-- 题目根容器 -->
<tex>...</tex>    <!-- 题干 -->
<as>...</as>      <!-- 答案 -->
<opt>...</opt>    <!-- 选项 -->
<jx>...</jx>      <!-- 解析 -->
```

### 样式标签（Paired - 成对）
```crqt
<str>text</str>   <!-- 加粗 -->
<ilt>text</ilt>   <!-- 倾斜 -->
<udl>text</udl>   <!-- 下划线 -->
```

### 状态标签（Self-closing - 自闭合）
```crqt
<fs>              <!-- 字号切换标记 -->
<ft>              <!-- 字体切换标记 -->
```

### 组件标签（Self-closing - 自闭合）
```crqt
<p>               <!-- 换行 -->
<hx>              <!-- 块级长横线 -->
<cd#1>            <!-- 填空#1（#可以是数字或*） -->
<cd#2>
<cd*>
<pt>              <!-- 插图路径 -->
```

### 字符标签（Self-closing - 自闭合）
```crqt
<uni>&#8212;</uni> <!-- Unicode转义 -->
```

## 使用指南

### 创建CRQT文档

1. 在VS Code中创建新文件，后缀名为`.crqt`
2. 开始使用CRQT标签
3. 自动获得语法高亮和括号匹配

### 基本结构

```crqt
<qt>
  <tex>
    题目文本内容。
    可以使用<str>加粗</str>、<ilt>倾斜</ilt>等样式。
    <cd#1/>处是要填空的地方。
  </tex>
  
  <opt>选项A</opt>
  <opt>选项B</opt>
  
  <as>
    参考答案
  </as>
  
  <jx>
    详细解析
  </jx>
</qt>
```

## 在VS Code中测试

1. 打开项目文件夹
2. 按 `F5` 键启动扩展开发主机
3. 在新的VS Code窗口中打开 `.crqt` 文件
4. 应该能看到语法高亮效果

## 打包和发布

要将此扩展打包为 `.vsix` 文件：

```bash
npm install -g vsce
vsce package
```

这会生成一个 `crqt-support-0.0.1.vsix` 文件。

## 主题定制

编辑 `themes/crqt-default-light.json` 文件可以自定义颜色方案：

```json
{
  "scope": "entity.name.tag.structure.crqt",
  "settings": {
    "foreground": "#0066CC"    // 改变颜色
  }
}
```

## 常见问题

**Q: 如何自定义标签的颜色？**
A: 编辑 `themes/crqt-default-light.json` 文件中的颜色值。

**Q: 如何添加新的标签类型？**
A: 
1. 在 `package.json` 中确保语言定义正确
2. 编辑 `syntaxes/crqt.tmLanguage.json` 添加新标签的匹配规则
3. 在 `themes/crqt-default-light.json` 中为新标签定义颜色

**Q: 为什么某些标签没有被着色？**
A: 检查 `syntaxes/crqt.tmLanguage.json` 中是否定义了该标签，以及主题文件中是否有相应的颜色定义。

## 贡献

欢迎提交问题报告和功能请求。

## 许可证

MIT
