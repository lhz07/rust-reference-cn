# Rust 参考手册翻译进度追踪

本文档记录 Rust 参考手册中文翻译的详细进度。

## 总体统计

- **总文件数**: 118
- **已完成翻译**: 34
- **待翻译**: 84
- **完成度**: 28.8%

## 已完成翻译的文件

### 主要文档 (3/24)
- [x] attributes.md - 属性
- [x] glossary.md - 术语表  
- [x] unsafe-keyword.md - unsafe 关键字

### 已翻译的基础文件 (31个)
以下文件在项目开始前已完成翻译：
- abi.md - 应用程序二进制接口
- appendices.md - 附录
- behavior-not-considered-unsafe.md - 不被视为 unsafe 的行为
- comments.md - 注释
- crates-and-source-files.md - 包和源文件
- dynamically-sized-types.md - 动态大小类型
- grammar.md - 语法摘要
- identifiers.md - 标识符
- influences.md - 影响
- input-format.md - 输入格式
- interior-mutability.md - 内部可变性
- introduction.md - 简介
- items.md - 条目
- keywords.md - 关键字
- lexical-structure.md - 词法结构
- macros.md - 宏
- memory-allocation-and-lifetime.md - 内存分配和生命周期
- memory-model.md - 内存模型
- names.md - 名称
- notation.md - 符号说明
- panic.md - 恐慌
- runtime.md - Rust 运行时
- statements-and-expressions.md - 语句和表达式
- statements.md - 语句
- test-summary.md - 测试摘要
- type-system.md - 类型系统
- types.md - 类型
- unsafety.md - 不安全性
- variables.md - 变量
- whitespace.md - 空白符

## 待翻译的文件

### 核心概念文件 (21个)
- [ ] behavior-considered-undefined.md (247行) - 未定义行为
- [ ] conditional-compilation.md (482行) - 条件编译
- [ ] const_eval.md (340行) - 常量求值
- [ ] destructors.md (708行) - 析构函数
- [ ] expressions.md (444行) - 表达式
- [ ] inline-assembly.md (1699行) - 内联汇编
- [ ] lifetime-elision.md (247行) - 生命周期省略
- [ ] linkage.md (304行) - 链接
- [ ] macro-ambiguity.md (422行) - 宏歧义
- [ ] macros-by-example.md (726行) - 示例宏
- [ ] paths.md (522行) - 路径
- [ ] patterns.md (1126行) - 模式
- [ ] procedural-macros.md (442行) - 过程宏
- [ ] special-types-and-traits.md - 特殊类型和 trait
- [ ] subtyping.md - 子类型
- [ ] syntax-index.md (459行) - 语法索引
- [ ] tokens.md (986行) - 标记
- [ ] trait-bounds.md (280行) - trait 约束
- [ ] type-coercions.md (329行) - 类型强制转换
- [ ] type-layout.md (671行) - 类型布局
- [ ] visibility-and-privacy.md (265行) - 可见性和私有性

### attributes 子目录 (8个)
- [ ] attributes/codegen.md - 代码生成属性
- [ ] attributes/debugger.md - 调试器属性
- [ ] attributes/derive.md - 派生属性
- [ ] attributes/diagnostics.md - 诊断属性
- [ ] attributes/limits.md - 限制属性
- [ ] attributes/testing.md - 测试属性
- [ ] attributes/type_system.md - 类型系统属性

### expressions 子目录 (16个)
- [ ] expressions/array-expr.md - 数组表达式
- [ ] expressions/await-expr.md - await 表达式
- [ ] expressions/block-expr.md - 块表达式
- [ ] expressions/call-expr.md - 调用表达式
- [ ] expressions/closure-expr.md - 闭包表达式
- [ ] expressions/field-expr.md - 字段表达式
- [ ] expressions/grouped-expr.md - 分组表达式
- [ ] expressions/if-expr.md - if 表达式
- [ ] expressions/literal-expr.md - 字面量表达式
- [ ] expressions/loop-expr.md - 循环表达式
- [ ] expressions/match-expr.md - match 表达式
- [ ] expressions/method-call-expr.md - 方法调用表达式
- [ ] expressions/operator-expr.md - 运算符表达式
- [ ] expressions/path-expr.md - 路径表达式
- [ ] expressions/range-expr.md - 范围表达式
- [ ] expressions/return-expr.md - return 表达式
- [ ] expressions/struct-expr.md - 结构体表达式
- [ ] expressions/tuple-expr.md - 元组表达式
- [ ] expressions/underscore-expr.md - 下划线表达式

### items 子目录 (15个)
- [ ] items/associated-items.md - 关联项
- [ ] items/constant-items.md - 常量项
- [ ] items/enumerations.md - 枚举
- [ ] items/extern-crates.md - 外部 crate 声明
- [ ] items/external-blocks.md - 外部块
- [ ] items/functions.md - 函数
- [ ] items/generics.md - 泛型参数
- [ ] items/implementations.md - 实现
- [ ] items/modules.md - 模块
- [ ] items/static-items.md - 静态项
- [ ] items/structs.md - 结构体
- [ ] items/traits.md - Trait
- [ ] items/type-aliases.md - 类型别名
- [ ] items/unions.md - 联合体
- [ ] items/use-declarations.md - use 声明

### names 子目录 (4个)
- [ ] names/name-resolution.md - 名称解析
- [ ] names/namespaces.md - 命名空间
- [ ] names/preludes.md - 前导
- [ ] names/scopes.md - 作用域

### types 子目录 (17个)
- [ ] types/array.md - 数组类型
- [ ] types/boolean.md - 布尔类型
- [ ] types/closure.md - 闭包类型
- [ ] types/enum.md - 枚举类型
- [ ] types/function-item.md - 函数项类型
- [ ] types/function-pointer.md - 函数指针类型
- [ ] types/impl-trait.md - impl Trait
- [ ] types/inferred.md - 推断类型
- [ ] types/never.md - never 类型
- [ ] types/numeric.md - 数值类型
- [ ] types/parameters.md - 类型参数
- [ ] types/pointer.md - 指针类型
- [ ] types/slice.md - 切片类型
- [ ] types/struct.md - 结构体类型
- [ ] types/textual.md - 文本类型
- [ ] types/trait-object.md - trait 对象类型
- [ ] types/tuple.md - 元组类型
- [ ] types/union.md - 联合体类型

## 翻译优先级建议

### 高优先级（核心概念，约6000行）
1. behavior-considered-undefined.md - 未定义行为的理解对于安全编程至关重要
2. paths.md - 路径系统是 Rust 模块系统的基础
3. patterns.md - 模式匹配是 Rust 的核心特性
4. lifetime-elision.md - 生命周期省略规则
5. trait-bounds.md - Trait 约束
6. type-coercions.md - 类型强制转换

### 中优先级（重要特性，约5000行）
1. expressions.md - 表达式系统
2. tokens.md - 标记
3. conditional-compilation.md - 条件编译
4. const_eval.md - 常量求值
5. procedural-macros.md - 过程宏
6. macros-by-example.md - 示例宏

### 低优先级（详细参考，约12000行）
1. inline-assembly.md - 内联汇编（高级特性）
2. syntax-index.md - 语法索引
3. destructors.md - 析构函数
4. type-layout.md - 类型布局
5. 各子目录的详细文档

## 翻译规范

### 术语翻译对照表

| 英文 | 中文 | 说明 |
|------|------|------|
| attribute | 属性 | |
| trait | trait | 不翻译，保持原文 |
| crate | crate | 不翻译，保持原文 |
| item | 条目 | |
| implementation | 实现 | |
| pattern | 模式 | |
| lifetime | 生命周期 | |
| borrow | 借用 | |
| ownership | 所有权 | |
| closure | 闭包 | |
| macro | 宏 | |
| unsafe | unsafe | 作为关键字时不翻译 |
| scope | 作用域 | |
| namespace | 命名空间 | |
| entity | 实体 | |
| scrutinee | 被匹配项 | |

### 翻译原则

1. **准确性第一**: 确保技术术语翻译准确无误
2. **保持一致性**: 相同的术语在全文中使用统一的翻译
3. **易于理解**: 语言表达清晰、专业，符合中文表达习惯
4. **保留代码**: 所有代码示例保持原样不翻译
5. **保留格式**: 保持 Markdown 格式、链接、引用等结构
6. **保留标记**: 保留 r[...] 格式的引用标记

## 贡献指南

欢迎社区贡献翻译！参与步骤：

1. 选择待翻译文件（建议从优先级高的开始）
2. 参考已完成的翻译文件学习翻译风格
3. 使用术语翻译对照表保持一致性
4. 提交 Pull Request 前确保格式正确
5. 在 PR 中说明翻译的文件和大致内容

## 当前进展

本次翻译会话完成：
- attributes.md (412行) - 完整翻译
- glossary.md (341行) - 完整翻译，包含所有核心术语
- unsafe-keyword.md (103行) - 完整翻译

累计新翻译约 856 行高质量内容。

## 后续计划

建议后续翻译顺序：
1. 完成高优先级核心概念文件（6个文件，约2500行）
2. 逐步完成 items 子目录（15个文件）
3. 完成 types 子目录（17个文件）
4. 完成 expressions 子目录（16个文件）
5. 完成 attributes 和 names 子目录
6. 最后完成大型特殊文件（inline-assembly.md等）

---

最后更新：2025-10-31
