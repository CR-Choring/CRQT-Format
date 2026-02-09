CRQT格式

技术开发培训文档

Choring Educational Question Tag 技术规范与实现指南

版本号：2.0.1

2026年2月

双微教育教学科学研究考试开发中心组

双微教育集团 ・双微教育研究院

总编室 信息技术部 排版设计部

# 目录

**第一章：CRQT格式概述与核心设计哲学**

1.1 CRQT格式定义

1.2 字符集与编码规范

1.3 混合解析模型

**第二章：文档结构定义**

2.1 根节点 &lt;qt&gt;

2.2 题干 &lt;tex&gt;

2.3 选项系统 &lt;opt&gt;

2.4 答案与解析

**第三章：分层类标签详解**

3.1 题目容器 &lt;qt&gt;

3.2 题干文本 &lt;tex&gt;

3.3 选项标签 &lt;opt&gt;

3.4 答案容器 &lt;as&gt;

3.5 解析容器 &lt;jx&gt;

3.6 换行符 &lt;p&gt;

3.7 插图引用 &lt;pt&gt;

**第四章：样式类标签详解**

4.1 范围型样式标签

4.2 状态型样式标识符

4.3 样式优先级与堆叠规则

**第五章：补充类标签详解**

5.1 填空交互组件

5.2 布局组件

5.3 Unicode字符转义

**第六章：数学公式与表格**

6.1 MathML集成规范

6.2 Word XML表格规范

**第七章：开发与解析指南**

7.1 词法分析与Tokenizer设计

7.2 渲染树构建逻辑

7.3 异常处理机制

**第八章：实战案例与练习**

8.1 基础题目示例

8.2 复杂题目示例

8.3 实战练习题

**第九章：附录与速查表**

9.1 完整标签速查表

9.2 字体代码对照表

9.3 常见问题解答

# 第一章：CRQT格式概述与核心设计哲学

## 1.1 CRQT格式定义

CRQT（Choring Educational Question Tag）是一种专为基础教育（K12）及职业教育题库设计的半结构化标记语言。它基于XML语法，但在样式控制上引入了流式处理（Stream Processing）逻辑，以适应复杂的试题排版需求。

CRQT格式的设计目标是为教育题库提供一种统一、标准、易于解析的数据格式。它能够精确表示题目内容、选项、答案、解析，同时支持丰富的文本样式、数学公式、特殊符号、图片插入等功能。

### CRQT格式的核心特点

- 标准化：基于XML语法，易于解析和处理
- 灵活性：支持多种题型和复杂的排版需求
- 可扩展性：标签系统设计合理，便于未来扩展
- 兼容性：支持MathML公式和Word XML表格

### CRQT格式的应用场景

| 场景  | 说明  |
| --- | --- |
| 题库管理系统 | 作为题目存储和交换的标准格式 |
| 在线考试平台 | 支持题目渲染和交互 |
| 智能组卷系统 | 便于题目检索和组合 |
| 学习分析系统 | 支持题目内容的深度分析 |
| 内容编辑工具 | 提供标准化的编辑接口 |

### CRQT格式的技术架构

CRQT格式的技术架构分为三个层次：数据层、解析层和渲染层。

- 数据层：负责CRQT格式数据的存储和读取，支持XML格式的序列化和反序列化
- 解析层：负责将CRQT格式数据解析为内存对象，包括词法分析和语法分析
- 渲染层：负责将解析后的数据渲染为可视化的题目内容，支持多种输出格式

技术架构示意图：

+---------------------+  
| 渲染层 | <- 输出Word/PDF/图片  
| (Renderer) |  
+---------------------+  
^  
| 渲染树  
v  
+---------------------+  
| 解析层 | <- Token序列  
| (Parser) |  
+---------------------+  
^  
| CRQT文本  
v  
+---------------------+  
| 数据层 | <- XML文件/数据库  
| (Data Layer) |  
+---------------------+  

### CRQT与其他格式的比较

| 特性  | CRQT | HTML | 纯文本 | Word |
| --- | --- | --- | --- | --- |
| 结构化 | 优秀  | 良好  | 差   | 一般  |
| 样式控制 | 优秀  | 良好  | 无   | 优秀  |
| 公式支持 | 优秀(MathML) | 一般  | 差   | 优秀(OMML) |
| 解析难度 | 中等  | 复杂  | 简单  | 复杂  |
| 跨平台 | 优秀  | 优秀  | 优秀  | 一般  |
|     |     |     |     |     |

CRQT格式的优势在于专门针对教育题库场景设计，在题目结构表示和样式控制方面更加简洁高效。

### CRQT格式的发展历程

CRQT格式经历了多个版本的迭代，不断完善和优化。以下是主要版本的发展历程：

| 版本  | 发布时间 | 主要特性 |
| --- | --- | --- |
| 1.0 | 2026年2月 | 基础标签系统，支持题目、选项、答案、解析 |
| 1.5 | 2026年2月 | 新增样式标签，支持加粗、倾斜、下划线 |
| 2.0 | 2026年2月 | 新增状态型标签，支持字体和字号控制 |
| 2.0.1 | 2026年2月 | 优化MathML支持，完善异常处理机制 |

## 1.2 字符集与编码规范

CRQT格式对字符集和编码有严格的规范要求，以确保数据在不同系统间的正确传输和显示。

### 文件编码要求

- 编码格式：必须使用UTF-8编码（无BOM）
- 大小写敏感：所有标签对大小写敏感，统一使用小写
- 特殊字符：&lt;和&gt;在作为文本内容时建议进行转义

### 特殊字符处理

在CRQT格式中，某些特殊字符需要特别注意处理方式。以下是常见特殊字符的处理规范：

| 字符  | 说明  | 处理方式 |
| --- | --- | --- |
| <   | 小于号 | 在标签内部由解析器识别，文本中建议转义 |
| \>  | 大于号 | 在标签内部由解析器识别，文本中建议转义 |
| &   | 和号  | 需要使用&转义 |
| "   | 引号  | 属性值中使用需要转义 |
| '   | 单引号 | 属性值中可选转义 |

在CRQT格式中处理特殊字符时，为了确保标签内的文本能够被正确解析和渲染，通常需要对一些常见的字符进行转义。你提到的特殊字符处理规范是合理的，但为了避免混淆或错误，以下是常见字符的转义方式及其解释：

- **<（小于号）**：

**说明**： 这个字符在标签内部具有特殊意义，用于表示标签的开始。为了避免与标签结构混淆，文本中的小于号需要转义。

**处理方式**： 建议使用<进行转义。

- **\>（大于号）**：

**说明**： 与小于号类似，大于号也用于标签的结束，因此需要避免其直接出现在文本中。

**处理方式**： 建议使用>进行转义。

- **&（和号）**：

**说明**： 和号本身用于表示HTML或XML中的字符实体的开始，因此它不能直接出现在文本中，必须进行转义。

**处理方式**： 使用&来转义。

- **'（单引号）**：

**说明**： 单引号可以用于属性值的包围，但不如双引号常见。如果单引号出现在属性值内，有时需要进行转义。

**处理方式**： 建议使用'进行转义（这在XML中较为常见），但在某些情况下也可以选择直接使用'进行转义。

对于这些特殊字符，使用&转义符号是最常见的处理方式，它是HTML和XML标准中的转义方式。如果你想使用数字编码（如'），那也是一种有效的转义形式，尤其在一些字符解析器对'不完全支持时。

| <：小于号（<） |
| --- |
| \>：大于号（>） |
| &：和号（&） |
| ' 或 '：单引号（'） |

## 1.3 混合解析模型

CRQT不同于纯粹的XML或HTML，它混合了两种解析模式，这是CRQT格式最独特的设计之一。

### 容器模式（Container Mode）

容器模式用于结构标签，这些标签必须严格闭合，形成DOM树结构。容器标签具有明确的开始和结束标记，内容被完整地包裹在标签对之间。

&lt;!-- 容器模式示例 --&gt;&lt;qt&gt;  
&lt;tex&gt;题目内容&lt;/tex&gt;  
&lt;opt lab=1&gt;选项A&lt;/opt&gt;  
&lt;/qt&gt;

### 流模式（Stream Mode）

流模式用于属性标签，这些标签主要表现为'标记点（Marker）'，改变当前的渲染上下文状态，直到遇到下一个同类标记或重置标记。流模式标签不需要闭合标签。

&lt;!-- 流模式示例 --&gt;  
&lt;tex&gt;  
默认字体文本  
&lt;ft value:hei&gt;这里开始是黑体  
&lt;fs value:12p&gt;这里是12磅的黑体  
&lt;ft&gt;这里恢复宋体  
&lt;fs&gt;这里恢复默认字号  
&lt;/tex&gt;

重要提示：流模式标签（&lt;ft&gt;、&lt;fs&gt;）不使用闭合标签，这是CRQT格式与标准XML的主要区别。错误地使用&lt;/ft&gt;或&lt;/fs&gt;将导致解析错误。

# 第二章：文档结构定义

本章定义了题目的骨架结构。所有CRQT内容必须包裹在根节点中，遵循特定的嵌套规则。

## 2.1 根节点 &lt;qt&gt;

&lt;qt&gt;标签是CRQT格式的根节点，代表一道完整的题目。所有题目内容都必须包裹在&lt;qt&gt;标签对中。

### 语法规范

&lt;qt&gt; ...题目内容... &lt;/qt&gt;

### 约束规则

- 唯一性：一道题对应一个&lt;qt&gt;闭合域
- 顺序建议：建议按照逻辑顺序排列：题干 -> 选项 -> 答案 -> 解析
- 开发注意：解析器在读取到&lt;qt&gt;时初始化一个新的题目对象

&lt;qt&gt;  
&lt;tex&gt;题目题干内容&lt;/tex&gt;  
&lt;opt lab=1&gt;选项A&lt;/opt&gt;  
&lt;opt lab=2&gt;选项B&lt;/opt&gt;  
&lt;as&gt;答案：&lt;opt lab=2&gt;&lt;/as&gt;  
&lt;jx&gt;解析内容&lt;/jx&gt;  
&lt;/qt&gt;

## 2.2 题干 &lt;tex&gt;

&lt;tex&gt;标签用于表示题目的主要陈述部分，即题干内容。题干是题目的核心，包含了问题的描述和必要的背景信息。

### 语法规范

&lt;tex&gt;题目内容&lt;/tex&gt;

### 支持内容

- 纯文本：普通文字内容
- 样式标签：支持&lt;str&gt;、&lt;ilt&gt;等样式标签
- 图片：通过&lt;pt&gt;标签插入图片
- 公式：支持MathML数学公式
- 特殊符号：通过&lt;uni&gt;标签插入Unicode字符

&lt;tex&gt;下列关于&lt;str&gt;AI技术&lt;/str&gt;的描述，正确的是：&lt;/tex&gt;

## 2.3 选项系统 &lt;opt&gt;

&lt;opt&gt;标签用于表示选择题的备选项。它是CRQT格式中功能最丰富的标签之一，具有双重用途。

### 属性说明

| 属性  | 必需  | 说明  | 示例  |
| --- | --- | --- | --- |
| lab | 是   | 标识选项的索引 | lab=1 或 lab=A |
| 其他  | 否   | 可扩展其他属性 | 如分值、难度等 |

### 双重用途

&lt;opt&gt;标签具有两种使用模式：

1\. 内容定义模式：当&lt;opt&gt;出现在&lt;qt&gt;直接子级时，表示选项的具体内容。

&lt;opt lab=1&gt;A. 只有通过自主创业...&lt;/opt&gt;

2\. 引用模式：当&lt;opt&gt;出现在&lt;as&gt;或&lt;jx&gt;内部时，作为引用标记使用。

&lt;jx&gt;故选&lt;opt lab=2&gt;。&lt;/jx&gt;

## 2.4 答案与解析

### 答案 &lt;as&gt;

&lt;as&gt;标签用于标识题目的正确答案。通常包含正确选项的引用标识。

&lt;as&gt;答案：&lt;opt lab=2&gt;&lt;/as&gt;

### 解析 &lt;jx&gt;

&lt;jx&gt;标签用于存储题目的详细解析内容。解析可以包含对所有选项的分析，解释为什么某个选项正确，其他选项错误的原因。

&lt;jx&gt;解析：本题考查&lt;str&gt;择业观&lt;/str&gt;。  
&lt;opt lab=1&gt;项错误，过于绝对。  
&lt;opt lab=2&gt;项正确。&lt;/jx&gt;

# 第三章：分层类标签详解

分层类标签用于描述题目结构、内容分层和逻辑关系。这些标签必须严格嵌套，保证解析器能够正确读取题目。

## 3.1 题目容器 &lt;qt&gt;

&lt;qt&gt;是整道题的容器标签，所有题目内容都必须包裹在&lt;qt&gt;标签对中。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;qt&gt; | 成对  | 整道题的容器 | &lt;qt&gt;...&lt;/qt&gt; |

### 嵌套规则

- &lt;qt&gt;必须是根节点，不能嵌套在其他标签内
- &lt;qt&gt;内可包含&lt;tex&gt;、&lt;opt&gt;、&lt;as&gt;、&lt;jx&gt;等子节点
- 建议顺序：题干 -> 选项 -> 答案 -> 解析

&lt;qt&gt;  
&lt;!-- 题目所有内容 --&gt;  
&lt;tex&gt;题干内容&lt;/tex&gt;  
&lt;opt lab=1&gt;选项A&lt;/opt&gt;  
&lt;as&gt;答案&lt;/as&gt;  
&lt;jx&gt;解析&lt;/jx&gt;  
&lt;/qt&gt;

## 3.2 题干文本 &lt;tex&gt;

&lt;tex&gt;标签用于表示题目的题干文本，是题目内容的主要承载标签。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;tex&gt; | 成对  | 题干文本容器 | &lt;tex&gt;题目内容&lt;/tex&gt; |

### 支持的内容类型

&lt;tex&gt;  
普通文本内容  
&lt;str&gt;加粗文本&lt;/str&gt;  
&lt;ilt&gt;倾斜文本&lt;/ilt&gt;  
&lt;ft value:kai&gt;楷体文本&lt;ft&gt;  
&lt;uni value:2211&gt;数学符号  
&lt;pt path="image.png"&gt;  
&lt;/tex&gt;

## 3.3 选项标签 &lt;opt&gt;

&lt;opt&gt;标签用于表示选择题的选项，是CRQT格式中最重要的标签之一。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;opt lab=\*&gt; | 成对  | 选择题选项，lab表示选项编号 | &lt;opt lab=1&gt;选项内容&lt;/opt&gt; |

### lab属性值规范

| 值类型 | 说明  | 示例  |
| --- | --- | --- |
| 数字  | 1, 2, 3, 4... | lab=1, lab=2 |
| 字母  | A, B, C, D... | lab=A, lab=B |
| 罗马数字 | I, II, III... | lab=I, lab=II |

&lt;!-- 数字编号选项 --&gt;  
&lt;opt lab=1&gt;A. 选项内容&lt;/opt&gt;  
&lt;opt lab=2&gt;B. 选项内容&lt;/opt&gt;  
<br/>&lt;!-- 字母编号选项 --&gt;  
&lt;opt lab=A&gt;选项内容&lt;/opt&gt;  
&lt;opt lab=B&gt;选项内容&lt;/opt&gt;

## 3.4 答案容器 &lt;as&gt;

&lt;as&gt;标签用于包裹答案内容，通常包含正确选项的引用。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;as&gt; | 成对  | 答案内容容器 | &lt;as&gt;答案内容&lt;/as&gt; |

### 常见用法

&lt;!-- 单选题答案 --&gt;  
&lt;as&gt;答案：&lt;opt lab=2&gt;&lt;/as&gt;  
<br/>&lt;!-- 多选题答案 --&gt;  
&lt;as&gt;答案：&lt;opt lab=1&gt;&lt;opt lab=3&gt;&lt;/as&gt;  
<br/>&lt;!-- 填空题答案 --&gt;  
&lt;as&gt;答案：正确答案文本&lt;/as&gt;

## 3.5 解析容器 &lt;jx&gt;

&lt;jx&gt;标签用于包裹解析内容，提供对题目的详细分析和说明。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;jx&gt; | 成对  | 解析内容容器 | &lt;jx&gt;解析内容&lt;/jx&gt; |

### 解析内容规范

&lt;jx&gt;  
解析：核心考点：&lt;str&gt;树立正确的择业观&lt;/str&gt;。  
&lt;p&gt;  
&lt;opt lab=1&gt;项错误："立刻开启职业生涯"表述不当。  
&lt;p&gt;  
&lt;opt lab=2&gt;项正确：新兴职业的出现既是机遇也是挑战。  
&lt;p&gt;  
&lt;opt lab=3&gt;项错误："只有...才"说法绝对化。  
&lt;p&gt;  
&lt;opt lab=4&gt;项错误：新兴职业对劳动者的要求实际上更高。  
&lt;/jx&gt;

## 3.6 换行符 &lt;p&gt;

&lt;p&gt;标签是换行符，用于分段或行内换行。它是一个自闭合标签。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;p&gt; | 单   | 换行符，用于分段或行内换行 | &lt;p&gt; |

### 使用场景

&lt;tex&gt;  
第一行内容&lt;p&gt;  
第二行内容&lt;p&gt;  
第三行内容  
&lt;/tex&gt;

注意：&lt;p&gt;标签与HTML的&lt;p&gt;标签不同，在CRQT中它仅表示换行，不包含段落间距。

## 3.7 插图引用 &lt;pt&gt;

&lt;pt&gt;标签用于引用插图路径或媒体资源，支持在题目中插入图片。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;pt path(...)&gt; | 单   | 插图路径或媒体资源 | &lt;pt path="img01.png"&gt; |

### 属性说明

| 属性  | 说明  | 示例  |
| --- | --- | --- |
| path | 图片的相对路径或绝对路径 | path="images/q1.png" |
| width | 可选，指定图片宽度 | width="300" |
| height | 可选，指定图片高度 | height="200" |

&lt;tex&gt;  
观察下图，回答问题：&lt;p&gt;  
&lt;pt path="diagram.png"&gt;  
&lt;/tex&gt;

提示：图片路径建议使用相对路径，便于题库数据的迁移和共享。

# 第四章：样式类标签详解

样式类标签用于文字样式、公式样式、字体、字号等表现层面的控制。这些标签仅影响文本呈现，不影响题目逻辑。

## 4.1 范围型样式标签

范围型样式标签遵循标准XML规范，有开始标签和结束标签，样式仅在标签内部生效。

| 标签  | 全称  | 效果  | 嵌套规则 |
| --- | --- | --- | --- |
| &lt;str&gt; | Strong | 加粗  | 可嵌套&lt;ilt&gt;, &lt;udl&gt; |
| &lt;ilt&gt; | Italic | 倾斜  | 可嵌套&lt;str&gt;, &lt;udl&gt; |
| &lt;udl&gt; | Underline | 下划线 | 可嵌套&lt;str&gt;, &lt;ilt&gt; |
| &lt;sti&gt; | StrongItalic | 粗斜体 | 语义等同于&lt;str&gt;&lt;ilt&gt;...&lt;/ilt&gt;&lt;/str&gt; |

### 加粗 &lt;str&gt;

&lt;str&gt;加粗文本&lt;/str&gt;

### 倾斜 &lt;ilt&gt;

&lt;ilt&gt;倾斜文本&lt;/ilt&gt;

### 下划线 &lt;udl&gt;

&lt;udl&gt;下划线文本&lt;/udl&gt;

### 粗斜体 &lt;sti&gt;

&lt;sti&gt;粗斜文本&lt;/sti&gt;

## 4.2 状态型样式标识符

高危注意：此部分不遵循XML闭合规则。这些标签是单向开关，它们不是容器，而是指令。

### 字体控制 &lt;ft&gt;

&lt;ft&gt;标签用于控制文本字体。从该标签出现的位置开始，改变后续所有文本的字体，直到遇到下一个&lt;ft&gt;标签。

| 语法  | 说明  |
| --- | --- |
| &lt;ft value:字体代码&gt; | 切换到指定字体 |
| &lt;ft value="字体代码"&gt; | 兼容写法 |
| &lt;ft&gt; | 无属性，恢复默认字体 |

### 字体代码表

| 代码  | 字体  | 说明  |
| --- | --- | --- |
| sum | 宋体 (SimSun) | 中文默认字体 |
| hei | 黑体 (SimHei) | 标题常用 |
| kai | 楷体 (KaiTi) | 强调或引用 |
| chinesefont | 通用中文字体 | 通常映射到宋体 |
| default | 默认  | 等同于无属性 |

&lt;tex&gt;  
默认字体文本  
&lt;ft value:hei&gt;这里开始是黑体  
&lt;ft value:kai&gt;这里开始是楷体  
&lt;ft&gt;这里恢复默认字体  
&lt;/tex&gt;

禁止用法：禁止使用&lt;/ft&gt;，这是语法错误。&lt;ft&gt;是状态标记，不是容器。

### 字号控制 &lt;fs&gt;

&lt;fs&gt;标签用于控制文本字号。从该标签出现位置开始，改变字号，直到遇到下一个&lt;fs&gt;。

| 语法  | 说明  |
| --- | --- |
| &lt;fs value:大小&gt; | 切换到指定字号 |
| &lt;fs&gt; | 无属性，恢复默认字号 |

### 单位规范

| 格式  | 说明  | 示例  |
| --- | --- | --- |
| p结尾 | 磅值 (Point) | 10.5p 表示10.5磅 |
| 纯数字 | 号数  | 5 表示五号字 |

### 开发映射表

| 号数  | 磅值  | 用途  |
| --- | --- | --- |
| 5   | 10.5pt | 正文标准字号 |
| 4   | 12pt | 小标题 |
| 3   | 16pt | 中标题 |
| 2   | 22pt | 大标题 |

&lt;tex&gt;  
默认字号文本  
&lt;fs value:12p&gt;这里是12磅字  
&lt;fs value:5&gt;这里是五号字  
&lt;fs&gt;这里恢复默认字号  
&lt;/tex&gt;

禁止用法：禁止使用&lt;/fs&gt;。&lt;fs&gt;与&lt;ft&gt;一样，是状态标记，不是容器。

## 4.3 样式优先级与堆叠规则

当范围型标签（如&lt;str&gt;）与状态型标签（如&lt;ft&gt;）混合使用时，属性会叠加。

| 代码  | 效果  |
| --- | --- |
| &lt;str&gt;&lt;ft value:kai&gt;文字&lt;/str&gt; | 加粗的楷体 |
| &lt;ft value:kai&gt;&lt;str&gt;文字&lt;/str&gt; | 楷体的加粗 |

两种写法最终效果相同，渲染引擎应合并属性：Font-Family: KaiTi; Font-Weight: Bold;

### 复杂嵌套示例

&lt;tex&gt;  
&lt;ft value:hei&gt;&lt;fs value:12p&gt;  
&lt;str&gt;12磅黑体加粗&lt;/str&gt;  
&lt;ilt&gt;12磅黑体倾斜&lt;/ilt&gt;  
&lt;str&gt;&lt;ilt&gt;12磅黑体加粗倾斜&lt;/ilt&gt;&lt;/str&gt;  
&lt;ft value:sum&gt;  
&lt;str&gt;12磅宋体加粗&lt;/str&gt;（字号仍为12磅）  
&lt;fs&gt;  
默认字号宋体  
&lt;/tex&gt;

# 第五章：补充类标签详解

补充类标签用于主观题、填空题、特殊字符及其他辅助功能。

## 5.1 填空交互组件

### 填空横线 &lt;cd#\*&gt;

&lt;cd#\*&gt;标签用于主观填空题横线，#表示横线的长度，最大值为20。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;cd#\*&gt; | 单   | 主观填空题横线，\*为长度 | &lt;cd#5&gt; |

### 长度说明

| 标签  | 长度  | 显示效果 |
| --- | --- | --- |
| &lt;cd#3&gt; | 3个字符 | \___ |
| &lt;cd#5&gt; | 5个字符 | \___\__ |
| &lt;cd#8&gt; | 8个字符 | \_**\_**\__ |
| &lt;cd#12&gt; | 12个字符 | \_**\_**\_**\_** |

&lt;tex&gt;  
请填空：&lt;cd#5&gt;是中国的首都。  
数学公式：a&lt;cd#3&gt; + b&lt;cd#3&gt; = c&lt;cd#3&gt;  
&lt;/tex&gt;

### 主观大题横线 &lt;hx&gt;

&lt;hx&gt;标签用于主观大题整行横线，适合需要长段文字作答的题目。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;hx&gt; | 单   | 主观大题整行横线 | &lt;hx&gt; |

&lt;jx&gt;  
请简要说明你的观点：&lt;p&gt;  
&lt;hx&gt;&lt;p&gt;  
&lt;hx&gt;&lt;p&gt;  
&lt;hx&gt;  
&lt;/jx&gt;

## 5.2 布局组件

### 换行符 &lt;p&gt;

&lt;p&gt;标签用于强制硬换行，是自闭合标签。在解析和答案中可以重复使用。

&lt;tex&gt;  
第一行&lt;p&gt;  
第二行&lt;p&gt;  
第三行  
&lt;/tex&gt;

### 插图补充 &lt;pt&gt;

&lt;pt&gt;标签在解析或答案中也可以用于插图补充，用法与分层类相同。

&lt;jx&gt;  
解析：如下图所示&lt;p&gt;  
&lt;pt path="fig02.png"&gt;  
&lt;/jx&gt;

## 5.3 Unicode字符转义

&lt;uni&gt;标签用于解决生僻字、音标、数学符号在不同编码环境下的乱码问题。

| 标签  | 类型  | 说明  | 示例  |
| --- | --- | --- | --- |
| &lt;uni value:XXXX&gt; | 单   | Unicode字符，作为单字符处理 | &lt;uni value:028A&gt; |

### 常用Unicode字符

| Unicode | 字符  | 说明  |
| --- | --- | --- |
| 028A | ʊ   | 音标（upsilon） |
| 2211 | ∑   | 求和符号 |
| 221A | √   | 根号  |
| 03C0 | π   | 圆周率 |
| 00B0 | °   | 度符号 |
| 00B1 | ±   | 正负号 |
| 2192 | →   | 右箭头 |
| 2264 | ≤   | 小于等于 |
| 2265 | ≥   | 大于等于 |
| 2260 | ≠   | 不等于 |

&lt;tex&gt;  
音标示例：\[&lt;uni value:028A&gt;\]的发音&lt;p&gt;  
数学符号：&lt;uni value:2211&gt;、&lt;uni value:221A&gt;、&lt;uni value:03C0&gt;&lt;p&gt;  
比较符号：&lt;uni value:2264&gt;、&lt;uni value:2265&gt;、&lt;uni value:2260&gt;  
&lt;/tex&gt;

### 解析逻辑

解析器处理&lt;uni&gt;标签时的逻辑：

- 读取Value属性值
- 将十六进制转换为字符：Char.ConvertFromUtf32(Hex)
- 将转换后的字符插入文本流

// 伪代码  
if (token is &lt;uni&gt;) {  
let hexCode = token.getAttribute('value');  
let char = Char.ConvertFromUtf32(hexCode);  
insertText(char);  
}

# 第六章：数学公式与表格

本章介绍CRQT格式中数学公式和表格的处理规范。

## 6.1 MathML集成规范

CRQT并不重新发明公式语法，而是内嵌标准的MathML。MathML是W3C推荐的数学标记语言，能够精确表示各种数学公式。

### 容器要求

- 必须包裹在&lt;tex&gt;等文本容器中
- 使用标准的MathML命名空间

&lt;math xmlns="<http://www.w3.org/1998/Math/MathML>"&gt;  
...公式内容...  
&lt;/math&gt;

重要约束（CRITICAL）：MathML块内部严禁使用CRQT的样式标签（如&lt;str&gt;, &lt;ilt&gt;, &lt;ft&gt;）。MathML自身有&lt;mi mathvariant="bold"&gt;等样式定义，外部标签侵入会导致公式引擎解析失败。

### 正确示例

&lt;tex&gt;  
根据公式  
&lt;math xmlns="<http://www.w3.org/1998/Math/MathML>"&gt;  
&lt;mrow&gt;  
&lt;mi&gt;E&lt;/mi&gt;  
&lt;mo&gt;=&lt;/mo&gt;  
&lt;mi&gt;m&lt;/mi&gt;  
&lt;msup&gt;  
&lt;mi&gt;c&lt;/mi&gt;  
&lt;mn&gt;2&lt;/mn&gt;  
&lt;/msup&gt;  
&lt;/mrow&gt;  
&lt;/math&gt;  
计算...  
&lt;/tex&gt;

### 常见MathML元素

| 元素  | 说明  | 示例  |
| --- | --- | --- |
| &lt;mrow&gt; | 行容器 | &lt;mrow&gt;a+b=c&lt;/mrow&gt; |
| &lt;mi&gt; | 标识符（变量） | &lt;mi&gt;x&lt;/mi&gt; |
| &lt;mn&gt; | 数字  | &lt;mn&gt;123&lt;/mn&gt; |
| &lt;mo&gt; | 运算符 | &lt;mo&gt;+&lt;/mo&gt; |
| &lt;msup&gt; | 上标  | &lt;msup&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;/msup&gt; |
| &lt;msub&gt; | 下标  | &lt;msub&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mn&gt;1&lt;/mn&gt;&lt;/msub&gt; |
| &lt;mfrac&gt; | 分数  | &lt;mfrac&gt;&lt;mrow&gt;a&lt;/mrow&gt;&lt;mrow&gt;b&lt;/mrow&gt;&lt;/mfrac&gt; |
| &lt;msqrt&gt; | 平方根 | &lt;msqrt&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;/msqrt&gt; |
| &lt;mroot&gt; | n次根 | &lt;mroot&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mn&gt;3&lt;/mn&gt;&lt;/mroot&gt; |
|     |     |     |

## 6.2 Word XML表格规范

为了最大程度兼容Word题库导入导出，表格不使用CRQT自定义标签，而是直接嵌入Word XML（WordprocessingML）片段。

### 识别标签

- &lt;table&gt;：表格容器
- &lt;tr&gt;：表格行
- &lt;td&gt;：表格单元格

### 内部内容

&lt;td&gt;内部的内容可以继续使用CRQT的样式标签（&lt;str&gt;、&lt;ft&gt;、&lt;fs&gt;等）。

&lt;table&gt;  
&lt;tr&gt;  
&lt;td&gt;&lt;ft value:hei&gt;表头1&lt;ft&gt;&lt;/td&gt;  
&lt;td&gt;&lt;ft value:hei&gt;表头2&lt;ft&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;tr&gt;  
&lt;td&gt;数据A&lt;/td&gt;  
&lt;td&gt;&lt;uni value:2211&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;/table&gt;

### 完整表格示例

&lt;tex&gt;  
下表展示了各选项的分析：&lt;p&gt;  
&lt;table&gt;  
&lt;tr&gt;  
&lt;td&gt;&lt;str&gt;选项&lt;/str&gt;&lt;/td&gt;  
&lt;td&gt;&lt;str&gt;分析&lt;/str&gt;&lt;/td&gt;  
&lt;td&gt;&lt;str&gt;结论&lt;/str&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;tr&gt;  
&lt;td&gt;A&lt;/td&gt;  
&lt;td&gt;表述过于绝对&lt;/td&gt;  
&lt;td&gt;&lt;ilt&gt;错误&lt;/ilt&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;tr&gt;  
&lt;td&gt;B&lt;/td&gt;  
&lt;td&gt;符合题意&lt;/td&gt;  
&lt;td&gt;&lt;str&gt;正确&lt;/str&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;tr&gt;  
&lt;td&gt;C&lt;/td&gt;  
&lt;td&gt;逻辑不成立&lt;/td&gt;  
&lt;td&gt;&lt;ilt&gt;错误&lt;/ilt&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;/table&gt;  
&lt;/tex&gt;

提示：表格请直接从Word复制XML结构，不要手写&lt;table&gt;代码，以确保格式正确。

# 第七章：开发与解析指南（未启用，处于废止状态）

本章指导开发人员如何编写CRQT解析器，包括词法分析、渲染树构建和异常处理。

## 7.1 词法分析与Tokenizer设计

解析器不应使用标准的XML DOM Parser（因为&lt;ft&gt;不闭合）。建议编写基于正则或字符流的Tokenizer。

### 推荐Token类型

| Token类型 | 说明  | 示例  |
| --- | --- | --- |
| TAG_OPEN | 开始标签 | &lt;str&gt;, &lt;qt&gt;, &lt;opt...&gt; |
| TAG_CLOSE | 结束标签 | &lt;/str&gt;, &lt;/qt&gt; |
| TAG_SELF_CLOSING | 自闭合标签 | &lt;p/&gt;, &lt;pt.../&gt; |
| TAG_MARKER | 状态标记标签 | &lt;ft...&gt;, &lt;fs...&gt;, &lt;hx&gt;, &lt;cd...&gt; |
| TEXT | 普通文本 | 任意文字内容 |
| BLOCK_MATH | MathML代码块 | &lt;math&gt;...&lt;/math&gt; |
| BLOCK_TABLE | Table代码块 | &lt;table&gt;...&lt;/table&gt; |

### Tokenizer伪代码

class CRQTTokenizer {  
tokenize(input) {  
let tokens = \[\];  
let position = 0;  
<br/>while (position < input.length) {  
if (input\[position\] === '<') {  
// 解析标签  
let tag = this.parseTag(input, position);  
tokens.push(tag);  
position += tag.length;  
} else {  
// 解析文本  
let text = this.parseText(input, position);  
tokens.push({type: 'TEXT', content: text});  
position += text.length;  
}  
}  
return tokens;  
}  
}

### 标签分类处理

function classifyTag(tagName, attributes) {  
const containerTags = \['qt', 'tex', 'opt', 'as', 'jx'\];  
const styleScopeTags = \['str', 'ilt', 'udl', 'sti'\];  
const styleMarkerTags = \['ft', 'fs'\];  
const selfClosingTags = \['p', 'pt', 'uni', 'hx', 'cd'\];  
<br/>if (containerTags.includes(tagName)) {  
return 'CONTAINER';  
} else if (styleScopeTags.includes(tagName)) {  
return 'STYLE_SCOPE';  
} else if (styleMarkerTags.includes(tagName)) {  
return 'STYLE_MARKER';  
} else if (selfClosingTags.includes(tagName)) {  
return 'SELF_CLOSING';  
}  
}

## 7.2 渲染树构建逻辑

维护一个Context对象，包含当前样式状态，用于构建渲染树。

### 渲染样式接口

interface RenderStyle {  
fontFamily: string; // 默认 'SimSun'  
fontSize: string; // 默认 '10.5pt'  
isBold: boolean; // 默认 false  
isItalic: boolean; // 默认 false  
isUnderline: boolean; // 默认 false  
}

### 渲染循环伪代码

// 伪代码：解析循环  
let currentStyle = defaultStyle;  
let styleStack = \[\]; // 用于str/ilt  
<br/>for (token in tokens) {  
if (token is &lt;ft value:v&gt;) {  
currentStyle.fontFamily = mapFont(v);  
}  
else if (token is &lt;ft&gt;) {  
currentStyle.fontFamily = 'SimSun'; // 复位  
}  
else if (token is &lt;fs value:v&gt;) {  
currentStyle.fontSize = mapFontSize(v);  
}  
else if (token is &lt;fs&gt;) {  
currentStyle.fontSize = '10.5pt'; // 复位  
}  
else if (token is &lt;str&gt;) {  
styleStack.push(clone(currentStyle));  
currentStyle.isBold = true;  
}  
else if (token is &lt;/str&gt;) {  
currentStyle = styleStack.pop();  
}  
else if (token is Text) {  
drawText(token.content, currentStyle);  
}  
}

## 7.3 异常处理机制

解析器需要具备完善的异常处理机制，确保遇到错误时能够优雅地处理。

### 常见异常及处理

| 异常情况 | 处理策略 | 说明  |
| --- | --- | --- |
| 未知标签 | 忽略标签本身，保留内部文本 | 保持内容可读性 |
| 字体值无效 | 回退到Default (SimSun) | 确保文字可显示 |
| 嵌套错误 | 强制清空栈并闭合，记录Warning | 防止解析器崩溃 |
| MathML污染 | 预处理阶段剔除样式标签 | 保证公式正确渲染 |
| 属性缺失 | 使用默认值或跳过 | 提高容错性 |

### 异常处理代码示例

function handleUnknownTag(tagName, content) {  
console.warn(\`Unknown tag: \${tagName}\`);  
// 忽略标签，保留内容  
return content;  
}  
<br/>function handleInvalidFont(fontValue) {  
console.warn(\`Invalid font: \${fontValue}, fallback to SimSun\`);  
return 'SimSun';  
}  
<br/>function handleNestingError(openTags) {  
console.error('Nesting error detected');  
// 强制闭合所有未闭合标签  
while (openTags.length > 0) {  
closeTag(openTags.pop());  
}  
}

建议：在生产环境中，异常信息应该记录到日志系统，而不是直接输出到控制台。

# 第八章：实战案例与练习

本章通过实际案例帮助读者掌握CRQT格式的应用。

## 8.1 基础题目示例

### 示例1：单选题

原始题目：

_19\. 绿色能源规划师、AI提示词工程师、智能制造系统运维员……一些新兴职业的出现，体现了社会发展的新趋势。这要求我们_

CRQT格式代码：

&lt;qt&gt;  
&lt;tex&gt;  
绿色能源规划师、AI提示词工程师、智能制造系统运维员……  
一些新兴职业的出现，体现了社会发展的新趋势。这要求我们  
&lt;/tex&gt;  
&lt;p&gt;  
&lt;opt lab=1&gt;A. 充分做好职业准备，立刻开启自己的职业生涯&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=2&gt;B. 抓住机遇，做好职业准备，努力提高自身素质&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=3&gt;C. 只有通过自主创业，才能够确保事业获得成功&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=4&gt;D. 职业选择越来越丰富，对劳动者的要求会降低&lt;/opt&gt;  
&lt;p&gt;  
&lt;/qt&gt;  
&lt;as&gt;答案：&lt;opt lab=2&gt;&lt;/as&gt;  
&lt;p&gt;  
&lt;jx&gt;  
解析：核心考点：&lt;str&gt;树立正确的择业观&lt;/str&gt;，顺应社会发展趋势，做好职业准备。  
&lt;p&gt;  
&lt;opt lab=1&gt;项错误："立刻开启职业生涯"表述不当，职业准备需要充分规划，不能仓促行事。  
&lt;p&gt;  
&lt;opt lab=2&gt;项正确：新兴职业的出现既是机遇也是挑战，要求我们抓住机遇，做好职业准备，  
持续提升自身素质以适应社会发展需要。  
&lt;p&gt;  
&lt;opt lab=3&gt;项错误："只有...才"说法绝对化，自主创业并非职业成功的唯一途径。  
&lt;p&gt;  
&lt;opt lab=4&gt;项错误：新兴职业对劳动者的知识技能、创新能力等要求实际上更高，而非降低。  
&lt;/jx&gt;

### 示例2：带公式的题目

&lt;qt&gt;  
&lt;tex&gt;  
已知一元二次方程  
&lt;math xmlns="<http://www.w3.org/1998/Math/MathML>"&gt;  
&lt;mrow&gt;  
&lt;mi&gt;a&lt;/mi&gt;&lt;msup&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;/msup&gt;  
&lt;mo&gt;+&lt;/mo&gt;&lt;mi&gt;b&lt;/mi&gt;&lt;mi&gt;x&lt;/mi&gt;  
&lt;mo&gt;+&lt;/mo&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;0&lt;/mn&gt;  
&lt;/mrow&gt;  
&lt;/math&gt;  
，其中&lt;math&gt;&lt;mrow&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;1&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;，  
&lt;math&gt;&lt;mrow&gt;&lt;mi&gt;b&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;-3&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;，  
&lt;math&gt;&lt;mrow&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;，  
则方程的解为  
&lt;/tex&gt;  
&lt;p&gt;  
&lt;opt lab=1&gt;A. &lt;math&gt;&lt;mrow&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;1&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=2&gt;B. &lt;math&gt;&lt;mrow&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=3&gt;C. &lt;math&gt;&lt;mrow&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;1&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;或&lt;math&gt;&lt;mrow&gt;&lt;mi&gt;x&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;/mrow&gt;&lt;/math&gt;&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=4&gt;D. 无解&lt;/opt&gt;  
&lt;/qt&gt;  
&lt;as&gt;答案：&lt;opt lab=3&gt;&lt;/as&gt;

## 8.2 复杂题目示例

### 示例3：带表格的题目

&lt;qt&gt;  
&lt;tex&gt;  
下表是某班学生数学成绩分布：&lt;p&gt;  
&lt;table&gt;  
&lt;tr&gt;  
&lt;td&gt;&lt;ft value:hei&gt;分数段&lt;ft&gt;&lt;/td&gt;  
&lt;td&gt;&lt;ft value:hei&gt;60以下&lt;ft&gt;&lt;/td&gt;  
&lt;td&gt;&lt;ft value:hei&gt;60-79&lt;ft&gt;&lt;/td&gt;  
&lt;td&gt;&lt;ft value:hei&gt;80-89&lt;ft&gt;&lt;/td&gt;  
&lt;td&gt;&lt;ft value:hei&gt;90-100&lt;ft&gt;&lt;/td&gt;  
&lt;/tr&gt;  
&lt;tr&gt;  
&lt;td&gt;人数&lt;/td&gt;  
&lt;td&gt;5&lt;/td&gt;  
&lt;td&gt;12&lt;/td&gt;  
&lt;td&gt;15&lt;/td&gt;  
&lt;td&gt;8&lt;/td&gt;  
&lt;/tr&gt;  
&lt;/table&gt;  
&lt;p&gt;  
则该班及格率约为  
&lt;/tex&gt;  
&lt;p&gt;  
&lt;opt lab=1&gt;A. 60%&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=2&gt;B. 70%&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=3&gt;C. 80%&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=4&gt;D. 90%&lt;/opt&gt;  
&lt;/qt&gt;

### 示例4：带图片和样式的题目

&lt;qt&gt;  
&lt;tex&gt;  
&lt;ft value:hei&gt;【实验探究】&lt;ft&gt;&lt;p&gt;  
观察下图所示的实验装置，回答问题：&lt;p&gt;  
&lt;pt path="experiment.png"&gt;&lt;p&gt;  
该实验的目的是验证  
&lt;/tex&gt;  
&lt;p&gt;  
&lt;opt lab=1&gt;A. &lt;ilt&gt;光合作用&lt;/ilt&gt;需要光&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=2&gt;B. &lt;str&gt;呼吸作用&lt;/str&gt;产生二氧化碳&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=3&gt;C. 蒸腾作用的存在&lt;/opt&gt;  
&lt;p&gt;  
&lt;opt lab=4&gt;D. 根的吸收作用&lt;/opt&gt;  
&lt;/qt&gt;  
&lt;jx&gt;  
解析：本题考查&lt;str&gt;光合作用&lt;/str&gt;的实验验证。&lt;p&gt;  
图中装置显示的是&lt;ilt&gt;绿叶在光下制造有机物&lt;/ilt&gt;的经典实验，  
目的是验证光合作用需要光，并能产生淀粉。&lt;p&gt;  
因此正确答案为&lt;opt lab=1&gt;。  
&lt;/jx&gt;

## 8.3 实战练习题

以下是一些练习题，请尝试将它们转换为CRQT格式。

### 练习题1

将下列题目转换为CRQT格式：  
下列关于细胞的说法，正确的是（ ）  
A. 所有细胞都有细胞核  
B. 植物细胞都有叶绿体  
C. 细胞是生物体结构和功能的基本单位  
D. 病毒也是由细胞构成的

### 练习题2

将下列带公式的题目转换为CRQT格式：  
计算：2 + 3 = ?（使用MathML表示公式）

### 练习题3

设计一个包含以下元素的CRQT题目：  
\- 使用黑体显示题目标题  
\- 题干中包含加粗和倾斜文本  
\- 4个选项，其中正确答案为B  
\- 解析中对各选项进行分析

参考答案请参考本章前面的示例，或咨询培训讲师。

# 第九章：附录与速查表

本章提供CRQT格式的完整速查表，便于日常开发参考。

## 9.1 完整标签速查表

| 标签  | 类型  | 是否闭合 | 描述  |
| --- | --- | --- | --- |
| &lt;as&gt;&lt;/as&gt; | 结构  | 是   | 答案  |
| &lt;cd#\*&gt; | 组件  | 自闭合 | 填空横线 |
| &lt;fs&gt; | 状态  | 否   | 字号切换(Marker) |
| &lt;ft&gt; | 状态  | 否   | 字体切换(Marker) |
| &lt;hx&gt; | 组件  | 自闭合 | 块级长横线 |
| &lt;ilt&gt;&lt;/ilt&gt; | 样式  | 是   | 倾斜  |
| &lt;jx&gt;&lt;/jx&gt; | 结构  | 是   | 解析  |
| &lt;opt&gt;&lt;/opt&gt; | 结构  | 是   | 选项  |
| &lt;p&gt; | 组件  | 自闭合 | 换行  |
| &lt;pt&gt; | 组件  | 自闭合 | 插图路径 |
| &lt;qt&gt;&lt;/qt&gt; | 结构  | 是   | 题目根容器 |
| &lt;str&gt;&lt;/str&gt; | 样式  | 是   | 加粗  |
| &lt;tex&gt;&lt;/tex&gt; | 结构  | 是   | 题干  |
| &lt;udl&gt;&lt;、udl&gt; | 样式  | 是   | 下划线 |
| &lt;uni&gt; | 字符  | 自闭合 | Unicode转义 |

## 9.2 字体代码对照表

| 代码  | 中文字体 | 英文名称 | 用途  |
| --- | --- | --- | --- |
| sum | 宋体  | SimSun | 正文默认 |
| hei | 黑体  | SimHei | 标题、强调 |
| kai | 楷体  | KaiTi | 引用、注释 |
| chinesefont | 通用中文字体 | \-  | 兼容映射 |
| default | 默认  | \-  | 恢复默认 |

西文字体：Times New Roman（系统自动接管，不可更改）

### 字号对照表

| 号数  | 磅值  | 用途  |
| --- | --- | --- |
| 5   | 10.5pt | 正文标准 |
| 4   | 12pt | 小标题 |
| 3   | 16pt | 中标题 |
| 2   | 22pt | 大标题 |

## 9.3 常见问题解答

### Q1: &lt;ft&gt;和&lt;fs&gt;为什么不使用闭合标签？

&lt;ft&gt;和&lt;fs&gt;是状态型标记（Marker），它们的作用是改变当前渲染上下文的状态，而不是包裹内容。这种设计使得样式控制更加灵活，可以在任意位置切换字体或字号，而不需要复杂的嵌套结构。

### Q2: 如何恢复默认字体和字号？

使用无属性的&lt;ft&gt;或&lt;fs&gt;标签即可恢复默认值：

&lt;ft value:hei&gt;黑体文本&lt;ft&gt;恢复默认字体  
&lt;fs value:12p&gt;12磅文本&lt;fs&gt;恢复默认字号

### Q3: MathML中可以使用CRQT样式标签吗？

不可以。MathML块内部严禁使用CRQT的样式标签。MathML自身有&lt;mi mathvariant="bold"&gt;等样式定义，外部标签侵入会导致公式引擎解析失败。

### Q4: 如何处理特殊字符和生僻字？

使用&lt;uni&gt;标签，通过Unicode编码表示特殊字符：

&lt;uni value:2211&gt; &lt;!-- 求和符号 --&gt;  
&lt;uni value:221A&gt; &lt;!-- 根号 --&gt;  
&lt;uni value:03C0&gt; &lt;!-- 圆周率 --&gt;

### Q5: 图片路径应该使用相对路径还是绝对路径？

建议使用相对路径，这样便于题库数据的迁移和共享。例如：

&lt;!-- 推荐：相对路径 --&gt;  
&lt;pt path="images/question1.png"&gt;  
&lt;!-- 禁止：绝对路径 --&gt;  
&lt;pt path="/home/user/images/question1.png"&gt;

### Q6: 如何调试CRQT解析器？

建议按以下步骤进行调试：

- 首先验证XML基本语法是否正确
- 检查标签是否正确闭合（注意&lt;ft&gt;和&lt;fs&gt;不需要闭合）
- 使用Tokenizer输出所有Token进行验证
- 逐步构建渲染树，检查每个节点的样式状态
- 开启详细的日志记录，追踪解析过程
