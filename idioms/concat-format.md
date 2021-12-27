# 用`format!`连接字符串 

## 说明

我们可以用`push` 和 `push_str`来向可变的`String`添加内容， 或者直接用`+`。
然而，使用`format!`通常会更加方便，特别是在字面literal和非字面non-literal字符串混合的情况。

## 示例

```rust
fn say_hello(name: &str) -> String {
    // We could construct the result string manually.
    // let mut result = "Hello ".to_owned();
    // result.push_str(name);
    // result.push('!');
    // result

    // But using format! is better.
    format!("Hello {}!", name)
}
```

## 优点

使用`format!`通常是组合字符串的最简洁和可读最好的方法。

## 缺点

它通常不是组合字符串的最高效的方法，对一个可变的字符串进行一系列的`push`操作一般是最高效的
（特别是当这个字符串已经预先分配了足够的内存）。
