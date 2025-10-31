r[notation]
# 符号说明

r[notation.grammar]
## 语法

r[notation.grammar.syntax]

*词法分析器*和*语法*部分的代码片段使用以下符号：

| 符号          | 示例                      | 含义                                   |
|-------------------|-------------------------------|-------------------------------------------|
| CAPITAL           | KW_IF, INTEGER_LITERAL        | 词法分析器生成的标记             |
| _ItalicCamelCase_ | _LetStatement_, _Item_        | 语法产生式                  |
| `string`          | `x`, `while`, `*`             | 准确的字符                    |
| x<sup>?</sup>     | `pub`<sup>?</sup>             | 可选项                          |
| x<sup>\*</sup>    | _OuterAttribute_<sup>\*</sup> | 0 个或多个 x                            |
| x<sup>+</sup>     |  _MacroMatch_<sup>+</sup>     | 1 个或多个 x                            |
| x<sup>a..b</sup>  | HEX_DIGIT<sup>1..6</sup>      | a 到 b 次重复 x                   |
| Rule1 Rule2       | `fn` _Name_ _Parameters_      | 按顺序的规则序列                |
| \|                | `u8` \| `u16`, Block \| Item  | 二选一                     |
| \[ ]               | \[`b` `B`]                     | 列出的任何字符              |
| \[ - ]             | \[`a`-`z`]                     | 范围内的任何字符        |
| ~\[ ]              | ~\[`b` `B`]                    | 任何字符，除了列出的       |
| ~`string`         | ~`\n`, ~`*/`                  | 任何字符，除了这个序列      |
| ( )               | (`,` _Parameter_)<sup>?</sup> | 分组项目                              |
| U+xxxx            | U+0060                        | 单个 Unicode 字符                |
| \<text\>          | \<any ASCII char except CR\>  | 对应匹配内容的英文描述 |
| Rule <sub>suffix</sub> | IDENTIFIER_OR_KEYWORD <sub>_except `crate`_</sub> | 对前一个规则的修改 |
| // 注释。 | // 单行注释。 | 延伸到行尾的注释。 |

序列的优先级高于 `|` 选择。

r[notation.grammar.string-tables]
### 字符串表产生式

语法中的某些规则 &mdash; 特别是[一元运算符][unary operators]、[二元运算符][binary operators]和[关键字][keywords] &mdash; 以简化形式给出：作为可打印字符串的列表。这些情况构成[词法单元][tokens]规则的子集，并假定是词法分析阶段馈送解析器的结果，由<abbr title="确定性有限自动机">DFA</abbr>驱动，对所有此类字符串表条目的析取进行操作。

当语法中出现这样的 `monospace` 字体字符串时，它是对这种字符串表产生式的单个成员的隐式引用。有关更多信息，请参阅[词法单元][tokens]。

r[notation.grammar.visualizations]
### 语法可视化

每个语法块下方都有一个按钮，用于切换[语法图][syntax diagram]的显示。方形元素是非终结符规则，圆角矩形是终结符。

[binary operators]: expressions/operator-expr.md#arithmetic-and-logical-binary-operators
[keywords]: keywords.md
[syntax diagram]: https://en.wikipedia.org/wiki/Syntax_diagram
[tokens]: tokens.md
[unary operators]: expressions/operator-expr.md#borrow-operators
