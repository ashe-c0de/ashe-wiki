First, let’s take a look at the ownership rules. Keep these rules in mind as we through the examples that illustrate them:
> Each value in Rust has an owner.
> There can only be one owner at a time.
> When the owner goes out of scope, the value will be dropped.


## Variable Scope
As a first example of ownership, we’ll look at the scope of some variables. A scope is the range within a program for which an item is valid. Take the following variable:
```rust
let s = “hello”;
```

The variable s refers to a string literal, where the value of the string is hardcoded into the text of our program. The variable is valid from the point at which it’s declared until the end of the current scope.
```rust
{

      let s = “hello”; // s is valid from this point forward

} // this scope is now over, and s is no longer valid
```

In other words, there are two important points in time here:

- When s comes into scope, it is valid.
-  It remains valid util it goes out of scope.

At this point, the relationship between scopes and when variables are valid is similar to that in other programming languages. Now we’ll build on top of this understanding by introducing the String type.

## The String Type

We’ll concentrate on the parts of String that relate to ownership. These aspects also apply to other complex data types, whether they are provided by the standard library or created by you.

 

We’ve already seen string literals, where a string value is hardcoded into our program. String literals are convenient, but they aren’t suitable for every situation in which we want to use text. One reason is that they’re immutable. Another is that not every string value can be known when we write our code: for example, what if we want to take user input and store it? For these situations, Rust has a second string type, String. This type manages data allocated on the heap and as such is able to store an amount of text that is unknown to us at compile time. You can create a String from a string literal using the from function, like so:

```rust
      let s = String::from(“hello”);
```
The double colon :: operator allows us to namespace this particular from function under the String type rather than using some sort of name sring_from.

 

This kind of string can be mutated:
```rust
      let mut s = String::from(“hello”);

      s.push_str(“, world!”);

      println!(“{}”, s);
```

So, what’s the difference here? Why can String be mutated but literals cannot? The difference is in how these types deal with memory.

## Memory and Allocation
In the case of a string literal, we know the contents at compile time, so the text is hardcoded directly into the final executable. This is why string literals are fast and efficient. But these properties only come from the string literal’s immutability. Unfortunately, we can’t put a blob of memory into the binary for each piece of text whose size is unknown at compile time and whose size might change while running the program.

 

With the String type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

- The memory must be requested from the memory allocator at runtime.
- We need a way of returning this memory to the allocator when we’re done with our String.

That first part is done by us: when we call String::from, its implementation requests the memory it needs. This is pretty much universal in programming languages.

 

However, the second part is different. In languages with garbage collector(GC), the GC keeps track of and cleans up memory that isn’t being used anymore, and we don’t need to think about it. In most languages without a GC, it’s our responsibility to identify when memory is no longer being used and call code to explicitly free it, just as we did to request it. Doing this correctly has historically been a difficult programming problem. If we forget, we’ll waste memory. If we do it too early, we’ll have an invalid variable. If we do it twice, that’s a bug too. We need to pair exactly one allocate with exactly one free.

 

Rust takes a different path: the memory is automatically return once the variable that owns it goes out of scope.

```rust
      {

            let s = String::from(“hello”); // s is valid from this point forward

      }  // this scope is now over, and s is no longer valid
```

There is a natural point at which we can return memory our String needs to the allocator: when s goes out of scope. When a variable goes out of scope, Rust calls a special function for us. This function is called drop, and it’s where the author of String can put the code to return the memory. Rust calls drop automatically at the closing curly bracket.

 

This pattern has a profound impact on the way Rust code is written. It may seem simple right now, but the behavior of code can be unexpected in more complicated situations when we want to have multiple variables use the data we’ve allocated on the heap. Let’s explore some of those situations now.

## Variables and Data Interacting with Move

Multiple variables can interact with the same data in different ways in Rust. Let’s look at an example using an integer

 ```rust
 let x = 5;
 let y = x;
```

We can probably guess what this is doing: “bind the value 5 to x; then make a copy of the value in x and bind it to y.” We now have two variables, x and y, and both equals 5. This is indeed what is happening, because integers are simple values with a known, fixed size, and these two 5 values are pushed onto the stack.

Now let’s look at the String version:

```rust
let s1 = String::from(“hello”);
let s2 = s1;
```
This looks very similar, so we might assume that the way it works would be the same: that is, the second line would make a copy of the value in s1 and bind it to s2. But this isn’t quite what happens.

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/rust/ownership-f1.png)
Figure-1

As shown in the above figure, a String is made up of three parts, shown on the left: a pointer to the memory that holds the contents of the string, a length, and a capacity. This group of data is stored on the stack. On the right is the memory on the heap that holds the contents.

 

The length is how much memory, in bytes, the contents of the string are currently using. The capacity is the total amount of memory, in bytes, that the String has received from the allocator. The difference between length and capacity matters, but not in this context, so for now, it’s fine to ignore the capacity.

 

When we assign s1 to s2, the String data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to. In other words, the data representation in memory looks like this:


![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/rust/ownership-f2.png)
Figure-2

Earlier, we said that when a variable goes out of scope, Rust automatically calls the drop function and cleans up the heap memory for that variable. But in the above figure shows both data pointers pointing to the same location. This is a problem: when s2 and s1 go out of scope, they will both try to free the same memory. This is known as a double free error and is one of the memory safety bugs we mentioned previously. Freeing memory twice can lead to memory corruption, which can potentially lead to security vulnerabilities.

 

To ensure memory safety, after the line let s2 = s1;, Rust considers s1 as no longer valid. Therefore, Rust doesn’t need to free anything when s1 goes out of scope. Check out what happens when you try to use s1 after s2 is created; it won’t work:
```rust
let s1 = String::from(“hello”);
let s2 = s1;
println!(“{}, world!”, s1);
```
You’ll get an error.

If you’ve heard the terms shallow copy and deep copy while working with other languages, the concept of copying the pointer, length, and capacity without coping the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a move. In this example, we would say that s1 was moved into s2. So, what actually happens is shown in the next figure:

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/rust/ownership-f3.png)
Figure-3

Representation in memory after s1 has been invalidated, that solves our problem! With only s2 valid, when it goes out of scope it alone will free the memory, and we’re done.

 

In addition, there’s a design choice that’s implied by this: Rust will never automatically create “deep” copies of your data. Therefore, any automatic copying can be assumed to be inexpensive in terms of runtime performance.

## Variables and Data Interacting with Clone
If we do want to deeply copy the heap data of the String, not just stack data, we can use a common method called clone. Here’s an example of the clone method in action:

```rust
      let s1 = String::from(“hello”);
      let s2 = s1.clone();
      println!(“s1 = {}, s2 = {}”, s1, s2);
```

This works just fine and explicitly produces the behavior shown in Figure-3, where the heap data does get copied.

 

When you see a call to clone, you know that some arbitrary code is being executed and that code may be expensive. It’s a visual indicator that something different is going on.

## Ownership and Functions

The mechanics of passing a value to a function are similar to those when assigning a value to a variable. Passing a variable to a function will move or copy, just as assignment does.

```rust
fn main() {
      let s = String::from(“hello”); // s comes into scope
      takes_ownership(s); // s’s value moves into the function…
               // …and so is no longer valid here
      let x = 5; // x comes into scope
      makes_copy(x); // x would move into the function, but i32 is copy, so it’s okay to still
                             // use x afterward
} // Here, x goes out scope, then s. But because s’s value was moved


fn takes_ownership(str: String) { // str comes into scope
      println!(“{}”, str);
} // str goes out of scope and `drop` is called. The backing memory is freed.


fn makes_copy(integer: i32) { // integer comes into scope
      println!(“{}”, integer);
} // Here, interger goes out of scope. Nothing special happens.
```

If we tried to use s after the call to takes_ownership, Rust would throw a compile-time error. These static checks protect us from mistakes. Try adding code to main that uses s and x to see where you can use them and where the ownership rules prevent you from doing so.

## Return Values and Scope

Returning values can also transfer ownership.
```rust
fn main() {
            let s1 = gives_ownership(); // gives_ownership moves its return value into s1
            let s2 = String::from(“hello”); // s2 comes into scope
            let s3 = takes_and_gives_back(s2); // s2 is moved into takes_and_gives_back, which also moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String { // gives_ownership will move its return value into the function that calls it
      let str = String::from(“yours”); // str comes into scope
      str // str is returned and moves out to the calling function
}

fn takes_and_gives_back(str: String) -> String { // str comes into scope
      str // str is returned and moves out to the calling function
}
```

The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it. When a variable that includes data on the heap goes out scope, the value will be cleaned up by drop unless ownership of the data has been moved to another variable.

 

While this works, taking ownership and then returning ownership with every function is a bit tedious. What if we want to let a function use a value but not take ownership? It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might to return as well.

 

Rust does let us return multiple values using a tuple:

```rust
fn main() {
    let s1 = String::from(“hello”);
    let (s2, len) = calculate_length(s1);
    println!(“The length of ‘{}’ is {}”, s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
      let length = s.len();
      (s, length)
}
```
But this is too much ceremony and a lot of work for a concept that should be common. Luckily for us, Rust has a feature for using a value without transferring ownership, called references.