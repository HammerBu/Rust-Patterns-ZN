# 使用借用类型作为参数

## 说明
当你决定使用哪种参数类型作为函数的参数时，尽量不要用带强制转换类型的引用，可以增加代码的灵活性。
通过这种方式，函数将接受更多的输入类型。

这种用法，并不局限于切片和胖指针。
事实上，你应该总是倾向于使用
__borrowed type__ 
而不是
__borrowing the owned type__.
例如 `&str` 而非 `&String`, `&[T]` 而非 `&Vec<T>` 以及 `&T` 而非 `&Box<T>`.

使用borrowed类型可以避免增加间接层，因为owned类型本来是对数据的封装，自带一个间接层。
例如，一个 `String` 有一层间接层，`&String`就成了两层间接层。
我们可以用`&str`进行替代，传入函数时`&String`强制转换成`&str`。

## 例子

在这个例子中, 我们将使用`&String`作为函数参数，与用 `&str` 进行对比，其中的区别跟 `&Vec<T>` 与`&[T]` 或者 `&Box<T>` 与 `&T` 都是类似的。

现在我们希望确定一个词是否包含三个连续的元音字母。
我们不必获得字符串的所有权，所以我们用引用。

代码如下:

```rust
fn three_vowels(word: &String) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let ferris = "Ferris".to_string();
    let curious = "Curious".to_string();
    println!("{}: {}", ferris, three_vowels(&ferris));
    println!("{}: {}", curious, three_vowels(&curious));

    // 上面的成功运行，下面的会报错
    // println!("Ferris: {}", three_vowels("Ferris"));
    // println!("Curious: {}", three_vowels("Curious"));

}
```

成功运行是因为我们传入的参数是`&String`类型。
后面注释掉的两行出错语句，错误原因是传入的参数成了`&str`，它并不能隐式自动转成`&String`类型。
改以下参数的类型就能解决这个问题。

比如我们将函数的声明变为如下：

```rust, ignore
fn three_vowels(word: &str) -> bool {
```

现在传入`&String`或者`&str`就都能正常运行了

```bash
Ferris: false
Curious: true
```

等等，还没完！书接下回，看到这你可能会撇撇嘴，然后说：那又怎样，我永远用不到`&'static str`
（比如上面例子中的`"Ferris"`）。
抛开上述静态字符串的情况，你仍然会发现使用`&str'会比使用`&String`更灵活。

考虑这样一个例子，有人给了我们一个句子，我们要确定句子中的每一个词是否包含三个连续的元音字母。
我们也许应该用刚刚写好的函数来对句子中的每个单词做判断。

代码如下：

```rust
fn three_vowels(word: &str) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let sentence_string =
        "Once upon a time, there was a friendly curious crab named Ferris".to_string();
    for word in sentence_string.split(' ') {
        if three_vowels(word) {
            println!("{} has three consecutive vowels!", word);
        }
    }
}
```

用传入参数为`&str`的版本可以正常运行，得到输出：

```bash
curious has three consecutive vowels!
```

然而，用`&String`参数的版本就不行。这是因为字符串的切片是`&str`而非`&String`，
转成`&String`并不是隐式的，而`String` 到 `&str`的转化却是高效并且隐式的。

## 参见

- [Rust Language Reference on Type Coercions](https://doc.rust-lang.org/reference/type-coercions.html)
- 更多关于如何使用`String` 和 `&str` 的讨论
  [this blog series (2015)](https://web.archive.org/web/20201112023149/https://hermanradtke.com/2015/05/03/string-vs-str-in-rust-functions.html)
  by Herman J. Radtke III
