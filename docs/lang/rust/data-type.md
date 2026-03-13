Every value in Rust is of a certain data type, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: scalarand compound.在Rust中每一个值都有确定的变量类型，以告知Rust使用的数据是哪一种指定的类型，Rust从而得知如何使用该数据，我们将看到两种数据类型子集：scalar和compound。

请牢记Rust是一门静态语言，这意味着在编译时就必须确定数据的类型。编译器通常可以根据数据的值或我们如何使用数据来推断其类型。比如：

```rust
// 关键字 变量名: 类型注释 = 变量值
let guess: u32 = "42".parse().expect("Not a number!");
```

if we don't add the : u32 type annotation shown in the preceding code, Rust will display the following error, which means the complier needs more imformation from us to know which type we want to use.

```bash
$ cargo build
   Compiling no_type_annotations v0.1.0 (file:///projects/no_type_annotations)
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |
help: consider giving `guess` an explicit type
  |
2 |     let guess: _ = "42".parse().expect("Not a number!");
  |              +++

For more information about this error, try `rustc --explain E0282`.
error: could not compile `no_type_annotations` due to previous error
```

## Scalar Types（标量类型）

A scalar type represents a single value. Rust has four primary scalar types:integers, floating-point numbers, Booleans, and characters. You may recognize these from other programming languages. Let’s jump into how they work in Rust.

### Integer types_整数型
| Length | Signed | Unsigned |
|--------|--------|----------|
|  8-bit |   i8   |    u8    |
| 16-bit |  i16   |   u16    |
| 32-bit |  i32   |   u32    |
| 64-bit |  i64   |   u64    |
|128-bit | i128   |  u128    |
|  arch  | isize  |  usize   |

Each variant can be either signed or unsigned and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative—in other words, whether the number needs to have a sign with it (signed) or whether it will only ever be positive and can therefore be represented without a sign (unsigned).每一种可以是有符号（具有正负标识）的或无符号的（不具有正负标识），并且具有明确的大小。有符号和无符号是指数字是否可能为负数，也就是说数字需要有符号 || 它只会是正数（因此可以无符号来表示）

### Floating-Point Types_浮点型
Rust also has two primitive types for floating-point numbers, which are numbers with decimal points.Rust存在两种浮点数类型（带小数点的数字），Rust’s floating-point types are f32 and f64, which are 32 bits and 64 bits in size, respectively. 分别是32位和64位。The default type is f64 because on modern CPUs, it’s roughly the same speed as f32 but is capable of more precision. All floating-point types are signed.默认64位，在现代CPU两者的速度相当且64位精度更高，两者都是带符号的。

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

### The Boolean Type_布尔型
As in most other programming languages, a Boolean type in Rust has two possible values: true and false. Booleans are one byte in size. The Boolean type in Rust is specified using bool.
```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

### The Character Type_字符型
Rust’s char type is the language’s most primitive alphabetic type.
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

## Compound Types（复合类型）
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.复合类型可以将众多数据组合一个类型，Rust有两种基本复合类型：元组 & 数组。

### The Tuple Type_元组
A tuple is a general way of grouping together a number of values with a variety of types into one compound type. 元组通常用于组合一系列不同类型的数据至某个复合类型中。Tuples have a fixed length: once declared, they cannot grow or shrink in size.元组拥有固定不变的长度，一旦被声明，size则不能增加或缩小。
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0; // 起始位为0

    let six_point_four = x.1;

    let one = x.2;
}
```

### The Array Type_数组
Another way to have a collection of multiple values is with an array. 另一种收集众多数据的方式即为数组。Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a fixed length.不同于元组，每个数组元素的类型必须相同。不同于其他语言，Rust中数组长度固定不变。

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```