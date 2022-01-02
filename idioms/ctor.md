# 构造器

## 说明

Rust语言没有直接提供构造器，通常用[关联函数associated function][]的`new`方法创建一个对象：

```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::new(42);
/// assert_eq!(42, s.value());
/// ```
pub struct Second {
    value: u64
}

impl Second {
    // 创建一个秒实例 Constructs a new instance of [`Second`].
    // 注意这是一个关联函数 - 没有self
    pub fn new(value: u64) -> Self {
        Self { value }
    }

    /// 返回以秒为单位的数值
    pub fn value(&self) -> u64 {
        self.value
    }
}
```

## Default

Rust提供了[`Default`][std-default]trait来实现默认构造函数：

```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
pub struct Second {
    value: u64
}

impl Second {
    /// 返回以秒为单位的数值
    pub fn value(&self) -> u64 {
        self.value
    }
}

impl Default for Second {
    fn default() -> Self {
        Self { value: 0 }
    }
}
```

如果每个字段都都实现了`Default`，即可派生得到整个实例的`Default`：

```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
#[derive(Default)]
pub struct Second {
    value: u64
}

impl Second {
    /// Returns the value in seconds.
    pub fn value(&self) -> u64 {
        self.value
    }
}
```

**注意：** 
当为一个类型实现`Default`时，既不需要也不建议同时提供一个没有参数的关联函数`new`。

**提示：** 
实现或派生`Default`后，你的类型可以用于需要`Default` 实现的地方，
尤其是对 [标准库中`*or_default`函数][std-or-default]的支持。

## 参见

- [Default 特性](default.md) 中对`Default` trait有更详尽的描述。

- [创建者模式](../patterns/creational/builder.md) 讨论了创建多配置multiple configurations对象。

[关联函数associated function]: https://doc.rust-lang.org/stable/book/ch05-03-method-syntax.html#associated-functions
[std-default]: https://doc.rust-lang.org/stable/std/default/trait.Default.html
[std-or-default]: https://doc.rust-lang.org/stable/std/?search=or_default
