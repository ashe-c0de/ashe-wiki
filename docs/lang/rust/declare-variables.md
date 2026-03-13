```
let mut guess = String::new();
```
在Rust中，一般使用let关键字keyword声明变量，比如
```
let apples = 5;
```
这行代码声明了一个名为apples的变量，并绑定其变量值为5，在Rust中变量默认是不可变的immutable。意味着一旦给变量赋值后，其值不会发生改变。声明可变的变量，在变量名之前加mut，比如
```
let apples = 5; // immutable
let mut bananas = 5; // mutable
```
::表明new是String类型的关联函数（associated function），关联函数是与类型相关联的，因此使用类型名称直接调用String::new()，它将创建一个新的、空的字符串实例。

Rustaceans say that the first variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable两个相同变量名的场景，第一个变量会被第二个变量遮蔽，意味着编译器看到的变量值是第二个（相同变量名）变量的值.
```
fn main() {
    let x = 5;

    let x = x + 1; // 6

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}"); // 12
    }

    println!("The value of x is: {x}"); // 6
}
```






常量constants 可以视为immutable（不可变）的变量，first，你不能对常量使用mut，因为它是默认带有mut的variable变量；second，声明常量的关键字是const而不是let，并且常量的类型必须被注释。

声明常量的示例：
```
// const 变量名: 类型 = 变量值;
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3; // Rust支持在runtime计算表达式的结果作为常量值
```



示例代码参考
```
fn main() {
    let x = 5;
    println!("The value of x is {x}");

    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
    println!("The value of THREE_HOURS_IN_SECONDS is {THREE_HOURS_IN_SECONDS}");
    
    const TWELVE: u32 = 12;
    println!("The value of TWELVE is {TWELVE}");

    let spaces = "   ";
    println!("The spaces is {spaces}");

    let spaces = spaces.len();
    println!("The spaces is {spaces}");
}
```
