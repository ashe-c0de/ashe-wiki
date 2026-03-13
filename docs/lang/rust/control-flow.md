在Rust中常用于流程控制的代码就是if expressions & loops

## if expressions
if expressions以关键字if开始，后面跟一个条件（布尔类型）。Optionally，we can also include an else expression, which we choose to do here, to give the program an alternative block of code to execute should the condition evaluate to false. if you don't provide an else expression and the condition is false, the program will just skip the if block and move on to next bit of code.

Because if is an expression, we can use it on the right side of a let statement to assign the outcome to a variable.由于if是表达式，因此我们可以将其返回的结果赋值给一个变量。

```rust
    // if expressions
    let number = 3;
    if number < 5 {
        println!("condition is true");
    } else {
        println!("condition is false");
    }

    
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
```

## Repetition with Loops
It's often useful to execute a block of code more than once. For this task, Rust provides several loops, which will run through the code inside the loop body to the end and then start immediately back at the beginning. Rust has three kinds of loops:`loop, while and for`.

The loop keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.
```rust
fn main() {
    // loop
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            // break可以结束循环并返回结果
            break counter * 2;
        }
    };

    println!("The result is {result}");
    
    // ----------------------------------------------------------------

    let mut count = 0;
    // 循环标签loop label，配合break使用，用以告知Rust结束的是哪一个循环
    // 在单重loop中，通常不使用loop label，break关键字单独使用时，默认结束当前循环体的循环
    'outer: loop {
        println!("count = {count}");
        let mut remaining = 10;

        'inner: loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break 'inner;
            }
            if count == 2{
                break 'outer;
            }
            remaining -= 1;
        }
        count += 1;
    }
    println!("End count = {count}");
}
```

## Conditional Loops with while
条件循环while
```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

## Looping Through a Collection with for
遍历集合的循环for
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```