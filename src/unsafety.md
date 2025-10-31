r[safety]
# 不安全性

r[safety.intro]
不安全操作是那些可能违反 Rust 静态语义的内存安全保证的操作。

r[safety.unsafe-ops]
以下语言级功能不能在 Rust 的安全子集中使用：

r[safety.unsafe-deref]
- 解引用[原始指针][raw pointer]。

r[safety.unsafe-static]
- 读取或写入[可变][mutable]或不安全的[外部][external]静态变量。

r[safety.unsafe-union-access]
- 访问 [`union`] 的字段，除了赋值给它。

r[safety.unsafe-call]
- 调用不安全函数。

r[safety.unsafe-target-feature-call]
- 从未启用相同功能的 `target_feature` 属性的函数调用标记有 [`target_feature`][attributes.codegen.target_feature] 的安全函数（参见 [attributes.codegen.target_feature.safety-restrictions]）。

r[safety.unsafe-impl]
- 实现[不安全特征][unsafe trait]。

r[safety.unsafe-extern]
- 声明 [`extern`] 块[^extern-2024]。

r[safety.unsafe-attribute]
- 将[不安全属性][unsafe attribute]应用于条目。

[^extern-2024]: 在 2024 版本之前，允许声明没有 `unsafe` 的 extern 块。

[`extern`]: items/external-blocks.md
[`union`]: items/unions.md
[mutable]: items/static-items.md#mutable-statics
[external]: items/external-blocks.md
[raw pointer]: types/pointer.md
[unsafe trait]: items/traits.md#unsafe-traits
[unsafe attribute]: attributes.md
