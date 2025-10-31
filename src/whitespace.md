r[lex.whitespace]
# 空白符

r[whitespace.syntax]
```grammar,lexer
@root WHITESPACE ->
      U+0009 // 水平制表符，`'\t'`
    | U+000A // 换行符，`'\n'`
    | U+000B // 垂直制表符
    | U+000C // 换页符
    | U+000D // 回车符，`'\r'`
    | U+0020 // 空格，`' '`
    | U+0085 // 下一行
    | U+200E // 从左到右标记
    | U+200F // 从右到左标记
    | U+2028 // 行分隔符
    | U+2029 // 段落分隔符

TAB -> U+0009 // 水平制表符，`'\t'`

LF -> U+000A  // 换行符，`'\n'`

CR -> U+000D  // 回车符，`'\r'`
```

r[lex.whitespace.intro]
空白符是仅包含具有[`Pattern_White_Space`] Unicode 属性的字符的任何非空字符串。

r[lex.whitespace.token-sep]
Rust 是一种"自由格式"语言，这意味着所有形式的空白符仅用于在语法中分隔_词法单元_，没有语义意义。

r[lex.whitespace.replacement]
如果将每个空白符元素替换为任何其他合法的空白符元素（例如单个空格字符），Rust 程序的含义完全相同。

[`Pattern_White_Space`]: https://www.unicode.org/reports/tr31/
