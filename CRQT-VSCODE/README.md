# CRQT Support

VS Code extension providing syntax highlighting for CRQT format - An XML-like markup language used for question/quiz templates.

## Features

Syntax highlighting for CRQT markup language with support for:

### Structure Tags (闭合)
- `<as></as>` - 答案 (Answer)
- `<jx></jx>` - 解析 (Parsing/Analysis)
- `<opt></opt>` - 选项 (Options)
- `<qt></qt>` - 题目根容器 (Question Root Container)
- `<tex></tex>` - 题干 (Question Stem)

### Style Tags (闭合)
- `<ilt></ilt>` - 倾斜 (Italic)
- `<str></str>` - 加粗 (Bold)
- `<udl></udl>` - 下划线 (Underline)

### Status Tags (Markers - 不闭合)
- `<fs>` - 字号切换 (Font Size Toggle)
- `<ft>` - 字体切换 (Font Type Toggle)

### Component Tags (自闭合)
- `<cd#*>` - 填空横线 (Fill-in-the-blank Line)
- `<hx>` - 块级长横线 (Block-level Long Line)
- `<p>` - 换行 (Line Break)
- `<pt>` - 插图路径 (Image Path)

### Character Tags (自闭合)
- `<uni>` - Unicode 转义 (Unicode Escape)

## Installation

1. Launch VS Code
2. Go to Extensions
3. Search for "CRQT Support"
4. Click Install

## Usage

Create a file with `.crqt` extension and start using CRQT markup:

```crqt
<qt>
  <tex>
    这是一个<str>加粗</str>的题目，有一个<ilt>倾斜</ilt>的单词。
    <cd#1>答案1</cd#1>
  </tex>
  <as>
    參考答案
  </as>
</qt>
```

## Example Document

```crqt
<qt>
  <tex>
    选择正确的答案：<p>
    <opt>选项1</opt><p>
    <opt>选项2</opt><p>
    <opt>选项3</opt>
  </tex>
  <as>
    答案：选项2
  </as>
  <jx>
    这是解释部分
  </jx>
</qt>
```

## Theme Colors

The extension utilizes VS Code's default color theme for syntax highlighting:
- Structure tags are highlighted in blue
- Style tags are highlighted in blue  
- Status markers are highlighted in orange/yellow
- Component tags are highlighted in purple
- Character tags are highlighted in red

## Requirements

VS Code 1.70.0 or higher

## Known Issues

None currently

## Release Notes

### 0.0.1

Initial release with CRQT language support

---
