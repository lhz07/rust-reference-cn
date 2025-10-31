r[items]
# 条目

r[items.syntax]
```grammar,items
Item ->
    OuterAttribute* ( VisItem | MacroItem )

VisItem ->
    Visibility?
    (
        Module
      | ExternCrate
      | UseDeclaration
      | Function
      | TypeAlias
      | Struct
      | Enumeration
      | Union
      | ConstantItem
      | StaticItem
      | Trait
      | Implementation
      | ExternBlock
    )

MacroItem ->
      MacroInvocationSemi
    | MacroRulesDefinition
```

r[items.intro]
_条目_是包的组成部分。条目在包内通过嵌套的[模块][modules]集合组织。每个包都有一个"最外层"的匿名模块；包内的所有其他条目在包的模块树中都有[路径][paths]。

r[items.static-def]
条目在编译时完全确定，在执行期间通常保持固定，并且可能驻留在只读内存中。

r[items.kinds]
有几种条目：

* [模块][modules]
* [`extern crate` 声明][`extern crate` declarations]
* [`use` 声明][`use` declarations]
* [函数定义][function definitions]
* [类型定义][type definitions]
* [结构体定义][struct definitions]
* [枚举定义][enumeration definitions]
* [联合体定义][union definitions]
* [常量项][constant items]
* [静态项][static items]
* [特征定义][trait definitions]
* [实现][implementations]
* [`extern` 块][`extern` blocks]

r[items.locations]
条目可以在[包的根][root of the crate]、[模块][modules]或[块表达式][block expression]中声明。

r[items.associated-locations]
称为[关联项][associated items]的条目子集可以在[特征][traits]和[实现][implementations]中声明。

r[items.extern-locations]
称为外部项的条目子集可以在 [`extern` 块][`extern` blocks]中声明。

r[items.decl-order]
条目可以按任何顺序定义，除了 [`macro_rules`]，它有自己的作用域行为。

r[items.name-resolution]
条目名称的[名称解析][Name resolution]允许在模块或块中引用条目之前或之后定义条目。

有关条目的作用域规则的信息，请参阅[条目作用域][item scopes]。

[`extern crate` declarations]: items/extern-crates.md
[`extern` blocks]: items/external-blocks.md
[`macro_rules`]: macros-by-example.md
[`use` declarations]: items/use-declarations.md
[associated items]: items/associated-items.md
[block expression]: expressions/block-expr.md
[constant items]: items/constant-items.md
[enumeration definitions]: items/enumerations.md
[function definitions]: items/functions.md
[implementations]: items/implementations.md
[item scopes]: names/scopes.md#item-scopes
[modules]: items/modules.md
[name resolution]: names/name-resolution.md
[paths]: paths.md
[root of the crate]: crates-and-source-files.md
[statement]: statements.md
[static items]: items/static-items.md
[struct definitions]: items/structs.md
[trait definitions]: items/traits.md
[traits]: items/traits.md
[type definitions]: items/type-aliases.md
[union definitions]: items/unions.md
