r[macro]
# 宏

r[macro.intro]
Rust 的功能和语法可以使用称为宏的自定义定义进行扩展。它们被赋予名称，并通过一致的语法调用：`some_extension!(...)`。

有两种方法可以定义新宏：

* [示例宏][Macros by Example]以更高级、声明性的方式定义新语法。
* [过程宏][Procedural Macros]使用对输入词法单元进行操作的函数定义类似函数的宏、自定义派生和自定义属性。

r[macro.invocation]
## 宏调用

r[macro.invocation.syntax]
```grammar,macros
MacroInvocation ->
    SimplePath `!` DelimTokenTree

DelimTokenTree ->
      `(` TokenTree* `)`
    | `[` TokenTree* `]`
    | `{` TokenTree* `}`

TokenTree ->
    Token _除了[分隔符][lex.token.delim]_ | DelimTokenTree

MacroInvocationSemi ->
      SimplePath `!` `(` TokenTree* `)` `;`
    | SimplePath `!` `[` TokenTree* `]` `;`
    | SimplePath `!` `{` TokenTree* `}`
```

r[macro.invocation.intro]
宏调用在编译时扩展宏，并将调用替换为宏的结果。宏可以在以下情况下调用：

r[macro.invocation.expr]
* [表达式][Expressions]和[语句][statements]

r[macro.invocation.pattern]
* [模式][Patterns]

r[macro.invocation.type]
* [类型][Types]

r[macro.invocation.item]
* [条目][Items]，包括[关联项][associated items]

r[macro.invocation.nested]
* [`macro_rules`] 转录器

r[macro.invocation.extern]
* [外部块][External blocks]

r[macro.invocation.item-statement]
当用作条目或语句时，使用 [MacroInvocationSemi] 形式，在不使用花括号时末尾需要分号。
[可见性限定符][Visibility qualifiers]永远不允许出现在宏调用或 [`macro_rules`] 定义之前。

```rust
// 用作表达式。
let x = vec![1,2,3];

// 用作语句。
println!("Hello!");

// 用在模式中。
macro_rules! pat {
    ($i:ident) => (Some($i))
}

if let pat!(x) = Some(1) {
    assert_eq!(x, 1);
}

// 用在类型中。
macro_rules! Tuple {
    { $A:ty, $B:ty } => { ($A, $B) };
}

type N2 = Tuple!(i32, i32);

// 用作条目。
# use std::cell::RefCell;
thread_local!(static FOO: RefCell<u32> = RefCell::new(1));

// 用作关联项。
macro_rules! const_maker {
    ($t:ty, $v:tt) => { const CONST: $t = $v; };
}
trait T {
    const_maker!{i32, 7}
}

// 宏中的宏调用。
macro_rules! example {
    () => { println!("Macro call in a macro!") };
}
// 首先扩展外部宏 `example`，然后扩展内部宏 `println`。
example!();
```

[Macros by Example]: macros-by-example.md
[Procedural Macros]: procedural-macros.md
[associated items]: items/associated-items.md
[delimiters]: tokens.md#delimiters
[expressions]: expressions.md
[items]: items.md
[`macro_rules`]: macros-by-example.md
[patterns]: patterns.md
[statements]: statements.md
[types]: types.md
[visibility qualifiers]: visibility-and-privacy.md
[External blocks]: items/external-blocks.md
