r[variable]
# 变量

r[variable.intro]
_变量_是栈帧的一个组成部分，可以是命名函数参数、匿名[临时值](expressions.md#temporaries)或命名局部变量。

r[variable.local]
_局部变量_（或*栈局部*分配）直接保存一个值，在栈的内存中分配。该值是栈帧的一部分。

r[variable.local-mut]
局部变量是不可变的，除非另有声明。例如：`let mut x = ...`。

r[variable.param-mut]
函数参数是不可变的，除非用 `mut` 声明。`mut` 关键字仅适用于后续参数。例如：`|mut x, y|` 和 `fn f(mut x: Box<i32>, y: Box<i32>)` 声明一个可变变量 `x` 和一个不可变变量 `y`。

r[variable.init]
局部变量在分配时未初始化。相反，整个帧的局部变量在帧进入时以未初始化状态分配。函数内的后续语句可能初始化或不初始化局部变量。局部变量只能在通过所有可达控制流路径初始化后才能使用。

在下一个示例中，`init_after_if` 在 [`if` 表达式][`if` expression]之后被初始化，而 `uninit_after_if` 没有，因为它在 `else` 情况下未初始化。

```rust
# fn random_bool() -> bool { true }
fn initialization_example() {
    let init_after_if: ();
    let uninit_after_if: ();

    if random_bool() {
        init_after_if = ();
        uninit_after_if = ();
    } else {
        init_after_if = ();
    }

    init_after_if; // ok
    // uninit_after_if; // err: use of possibly uninitialized `uninit_after_if`
}
```

[`if` expression]: expressions/if-expr.md#if-expressions
