# `Default` 特性

## 说明

Rust中的许多类型都有一个[构造器]。
但这些构造器都是针对特定类型的，Rust没有抽象出一个对所有类型适用的`new()`方法。
因此[`Default`] 特性trait应运而生，它适用于容器containers或者其他泛型类型generic types
（例如[`Option::unwrap_or_default()`]）
值得注意的是，一些容器containers已经实现了`Default`特性。

不仅单例容器如`Cow`, `Box` 和 `Arc` 实现了`Default`，
使用`#[derive(Default)]`可以自动为所含字段实现了`Default`的结构体structs实现`Default`。
所以越多的类型支持Default`，它就会越有用。

然而，构造器可以用不同的参数去构建实例，但`default()`不行。
我们可以定义多个不同的构造器去构造实例，但是`Default`的只能有一种实现。

## 例子

```rust
use std::{path::PathBuf, time::Duration};

// 这里我们可以很方便地自动实现 Default
#[derive(Default, Debug, PartialEq)]
struct MyConfiguration {
    // Option类型默认为 None
    output: Option<PathBuf>,
    // Vecs类型默认为空 vector
    search_path: Vec<PathBuf>,
    // Duration类型默认为0
    timeout: Duration,
    // bool类型默认为 false
    check: bool,
}

impl MyConfiguration {
    // 这里添加 setters
}

fn main() {
    // 用默认值构建一个实例
    let mut conf = MyConfiguration::default();
    // 改动一些键值
    conf.check = true;
    println!("conf = {:#?}", conf);
        
    // 用部分初始化partial initialization进行初始化
    let conf1 = MyConfiguration {
        check: true,
        ..Default::default()
    };
    assert_eq!(conf, conf1);
}
```

## 参见

- [构造器] 创建实例的方法，可以是默认的"default"，也可以不是
- [`Default`] 文档 (下拉查看各实现implementors)
- [`Option::unwrap_or_default()`]
- [`derive(new)`]

[构造器]: ctor.md
[`Default`]: https://doc.rust-lang.org/stable/std/default/trait.Default.html
[`Option::unwrap_or_default()`]: https://doc.rust-lang.org/stable/std/option/enum.Option.html#method.unwrap_or_default
[`derive(new)`]: https://crates.io/crates/derive-new/
