r[ident]
# 标识符

r[ident.syntax]
```grammar,lexer
IDENTIFIER_OR_KEYWORD -> ( XID_Start | `_` ) XID_Continue*

XID_Start -> <由 Unicode 定义的 `XID_Start`>

XID_Continue -> <由 Unicode 定义的 `XID_Continue`>

RAW_IDENTIFIER -> `r#` IDENTIFIER_OR_KEYWORD

NON_KEYWORD_IDENTIFIER -> IDENTIFIER_OR_KEYWORD _除了[严格][lex.keywords.strict]或[保留][lex.keywords.reserved]关键字_

IDENTIFIER -> NON_KEYWORD_IDENTIFIER | RAW_IDENTIFIER

RESERVED_RAW_IDENTIFIER -> `r#` (`_` | `crate` | `self` | `Self` | `super`)
```

<!-- 更新版本时，也要更新 UAX 链接。 -->
r[ident.unicode]
标识符遵循 [Unicode 标准附录 #31][UAX31] 中 Unicode 版本 16.0 的规范，并添加了下面描述的内容。一些标识符示例：

* `foo`
* `_identifier`
* `r#true`
* `Москва`
* `東京`

r[ident.profile]
使用的 UAX #31 配置文件是：

* Start := [`XID_Start`]，加上下划线字符 (U+005F)
* Continue := [`XID_Continue`]
* Medial := 空

> [!NOTE]
> 以下划线开头的标识符通常用于指示有意未使用的标识符，并将使 `rustc` 中的未使用警告静音。

r[ident.keyword]
没有下面[原始标识符](#raw-identifiers)中描述的 `r#` 前缀，标识符不能是[严格][strict]或[保留][reserved]关键字。

r[ident.zero-width-chars]
标识符中不允许使用零宽度非连接符（ZWNJ U+200C）和零宽度连接符（ZWJ U+200D）字符。

r[ident.ascii-limitations]
在以下情况下，标识符仅限于 [`XID_Start`] 和 [`XID_Continue`] 的 ASCII 子集：

* [`extern crate`] 声明（除了 [AsClause] 标识符）
* 在[路径][path]中引用的外部包名称
* 从文件系统加载的没有 [`path` 属性][`path` attribute]的[模块][Module]名称
* [`no_mangle`] 属性项
* [外部块][external blocks]中的条目名称

r[ident.normalization]
## 规范化

标识符使用 [Unicode 标准附录 #15][UAX15] 中定义的规范化形式 C (NFC) 进行规范化。如果两个标识符的 NFC 形式相等，则它们相等。

[过程宏][proc-macro]和[声明宏][mbe]在其输入中接收规范化的标识符。

r[ident.raw]
## 原始标识符

r[ident.raw.intro]
原始标识符类似于普通标识符，但前缀为 `r#`。（注意 `r#` 前缀不包括在实际标识符中。）

r[ident.raw.allowed]
与普通标识符不同，原始标识符可以是任何严格或保留关键字，除了上面为 `RAW_IDENTIFIER` 列出的关键字。

r[ident.raw.reserved]
使用 [RESERVED_RAW_IDENTIFIER] 标记是错误的。

[`extern crate`]: items/extern-crates.md
[`no_mangle`]: abi.md#the-no_mangle-attribute
[`path` attribute]: items/modules.md#the-path-attribute
[`XID_Continue`]: http://unicode.org/cldr/utility/list-unicodeset.jsp?a=%5B%3AXID_Continue%3A%5D&abb=on&g=&i=
[`XID_Start`]:  http://unicode.org/cldr/utility/list-unicodeset.jsp?a=%5B%3AXID_Start%3A%5D&abb=on&g=&i=
[external blocks]: items/external-blocks.md
[mbe]: macros-by-example.md
[module]: items/modules.md
[path]: paths.md
[proc-macro]: procedural-macros.md
[reserved]: keywords.md#reserved-keywords
[strict]: keywords.md#strict-keywords
[UAX15]: https://www.unicode.org/reports/tr15/tr15-56.html
[UAX31]: https://www.unicode.org/reports/tr31/tr31-41.html
