Functions are prevalent in Rust code. You’ve already seen one of the most important functions in the language: the main function, which is the entry point of many programs. You’ve also seen the fn keyword, which allows you to declare new functions.函数在Rust代码中非常流行，你已经见过最重要的函数——main函数（许多编程语言的入口），你也见过fn关键字，其允许你用以声明一个新的函数。

Rust code uses snake case as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words. Rust代码以蛇形命名法来命名函数名和变量名，其形式由小写字母+下划线组成。

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

// fn关键字 函数名 (形参列表) {函数体}
fn another_function() {
    println!("Another function.");
}
```

## Parameters_参数
We can define functions to have parameters, which are special variables that are part of a function's signature. 我们可以给函数定义一些参数，这些指定的变量是函数签名的一部分。When a function has parameters, you can provide it with concrete values for those parameters. 当函数具有参数时，你可以为这些参数提供具体的值。Technically, the concrete values are called arguments, but in casual conversation, people ten to use the words parameter and argument interchangeably for either the variables in a function's definition or the concrete values passed in when you call a funcion.从技术上来讲，这些具体的值应称之为arguments，但日常称谓中，人们将parameter和argument混淆使用，来表示定义函数的参数parameter或调用函数时提供的参数argument。

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

## Statements and Expressions
Function bodies are made up of a series of statements optionally ending in an expression.函数体由一系列语句+表达式结尾（可选）组成。 // 函数体由花括号包裹

statements: instructions that perform some action and do not return a value. // 执行某些操作的指令但不返回值

expressions: evaluate to a resultant value. // 返回值

```rust
// 有返回值函数
// fn func_name () -> type {body}
fn five() -> i32 {
    5
}

// 无返回值函数
fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```

```rust
fn main() {
    let x = plus_one(2);
    println!("The value of x is {x}");
}

// 此函数有返回值类型，因此最后一句必须为expressions
fn plus_one(x: i32) -> i32 {
//    x + 1; // 错误写法，编译报错
    x + 1 // 正确写法，表达式expressions不以分号';'结尾，语句statements需要有分号结尾
}
```