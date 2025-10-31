r[statement]
# 语句

r[statement.syntax]
```grammar,statements
Statement ->
      `;`
    | Item
    | LetStatement
    | ExpressionStatement
    | OuterAttribute* MacroInvocationSemi
```

r[statement.intro]
*语句*是[块][block]的组成部分，而块又是外部[表达式][expression]或[函数][function]的组成部分。

r[statement.kind]
Rust 有两种语句：[声明语句](#declaration-statements)和[表达式语句](#expression-statements)。

r[statement.decl]
## 声明语句

*声明语句*是将一个或多个*名称*引入封闭语句块的语句。
声明的名称可以表示新变量或新[条目][item]。

声明语句有两种：条目声明和 `let` 语句。

r[statement.item]
### 条目声明

r[statement.item.intro]
*条目声明语句*的语法形式与[模块][module]内的[条目声明][item]相同。

r[statement.item.scope]
在语句块内声明条目会将其[作用域][scope]限制为包含该语句的块。
该条目不会被赋予[规范路径][canonical path]，其声明的任何子条目也不会。

r[statement.item.associated-scope]
例外的是，只要条目和（如果适用）特征可访问，[实现][implementations]定义的关联项在外部作用域中仍然可访问。
否则，它在含义上与在模块内声明条目相同。

r[statement.item.outer-generics]
不存在对包含函数的泛型参数、参数和局部变量的隐式捕获。
例如，`inner` 不能访问 `outer_var`。

```rust
fn outer() {
  let outer_var = true;

  fn inner() { /* outer_var 在这里不在作用域内 */ }

  inner();
}
```

r[statement.let]
### `let` 语句

r[statement.let.syntax]
```grammar,statements
LetStatement ->
    OuterAttribute* `let` PatternNoTopAlt ( `:` Type )?
    (
          `=` Expression
        | `=` Expression _除了 [LazyBooleanExpression] 或以 `}` 结尾_ `else` BlockExpression
    )? `;`
```

r[statement.let.intro]
*`let` 语句*通过[模式][pattern]引入一组新的[变量][variables]。
模式后面可选地跟随类型注释，然后要么结束，要么后跟初始化表达式加上可选的 `else` 块。

r[statement.let.inference]
当没有给出类型注释时，编译器将推断类型，或者如果没有足够的类型信息可用于明确推断，则发出错误信号。

r[statement.let.scope]
由变量声明引入的任何变量从声明点到封闭块作用域的末尾可见，除非它们被另一个变量声明遮蔽。

r[statement.let.constraint]
如果 `else` 块不存在，则模式必须是不可反驳的。
如果 `else` 块存在，则模式可以是可反驳的。

r[statement.let.behavior]
如果模式不匹配（这要求它是可反驳的），则执行 `else` 块。
`else` 块必须始终发散（求值为 [never 类型][never type]）。

```rust
let (mut v, w) = (vec![1, 2, 3], 42); // 绑定可以是 mut 或 const
let Some(t) = v.pop() else { // 可反驳的模式需要 else 块
    panic!(); // else 块必须发散
};
let [u, v] = [v[0], v[1]] else { // 这个模式是不可反驳的，所以编译器
                                 // 会发出警告，因为 else 块是多余的。
    panic!();
};
```

r[statement.expr]
## 表达式语句

r[statement.expr.syntax]
```grammar,statements
ExpressionStatement ->
      ExpressionWithoutBlock `;`
    | ExpressionWithBlock `;`?
```

r[statement.expr.intro]
*表达式语句*是求值[表达式][expression]并忽略其结果的语句。
通常，表达式语句的目的是触发求值其表达式的效果。

r[statement.expr.restriction-semicolon]
仅由[块表达式][block]或控制流表达式组成的表达式，如果在允许语句的上下文中使用，可以省略尾随分号。
这可能会导致它被解析为独立语句还是另一个表达式的一部分之间的歧义；
在这种情况下，它被解析为语句。

r[statement.expr.constraint-block]
当用作语句时，[ExpressionWithBlock] 表达式的类型必须是单元类型。

```rust
# let mut v = vec![1, 2, 3];
v.pop();          // 忽略从 pop 返回的元素
if v.is_empty() {
    v.push(5);
} else {
    v.remove(0);
}                 // 可以省略分号。
[1];              // 独立的表达式语句，不是索引表达式。
```

当省略尾随分号时，结果必须是类型 `()`。

```rust
// 错误：块的类型是 i32，不是 ()
// 错误：由于默认返回类型，期望 `()`
// if true {
//   1
// }

// 正确：块的类型是 i32
if true {
  1
} else {
  2
};
```

r[statement.attribute]
## 语句上的属性

语句接受[外部属性][outer attributes]。
对语句有意义的属性是 [`cfg`] 和[检查属性][the lint check attributes]。

[block]: expressions/block-expr.md
[expression]: expressions.md
[function]: items/functions.md
[item]: items.md
[module]: items/modules.md
[never type]: types/never.md
[canonical path]: paths.md#canonical-paths
[implementations]: items/implementations.md
[variables]: variables.md
[outer attributes]: attributes.md
[`cfg`]: conditional-compilation.md
[the lint check attributes]: attributes/diagnostics.md#lint-check-attributes
[pattern]: patterns.md
[scope]: names/scopes.md
