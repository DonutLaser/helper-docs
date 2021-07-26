# Comments
Line comments are standard `//`  
Documentation comments are `///` and support markdown notation

# Types
`i8/16/32/64/128` - 8/16/32/64/128-bit signed integer
`u8/16/32/64/128` - 8/16/32/64/128-bit unsigned integer
`f32/64` - 32/64-bit float
`bool` - boolean
`char` - single character. Represents a **unicode** value.

An `enum`:
```
enum Direction { Left, Right, Up, Down };
let up = Direction::Up;
```

An `enum with fields`. When pattern matching, if a variable is equal to the enum value with a field, the field value will be the result of that match:
```
enum OptionalI32 {
    AnI32(i32),
    Nothing,
}
let two: OptionalI32 = OptionalI32::AnI32(2);
let nothing = OptionalI32::Nothing;
```

A `struct`:
```
struct Point {
    x: i32,
    y: i32,
}
let origin: Point = Point { x: 0, y : 0 }; // Initialize a variable of type Poin
```

A `tuple struct`:
```
struct Point2(i32, i32);
let origin2 = Point2(0, 0); // Initialize a variable of type Point2
```

# Variables
By default, variables are immutable (unable to change after declaration) and declared with `let`. Variable type can be inferred most of the time, so no need to specify the type explicitly.   
To create a variable:
```
let x: i32 = 1; // Immutable
```

To create a mutable variable, use `mut` keyword:
```
let mut mutable = 1;
mutable = 4;
mutable += 2;
```

To create a const variable, use `const` keyword:
```
const MAX_POINTS: u32 = 100_000;
```

You cannot write the following:
```
let x = y = 5;
```

### Differences between immutable variables and constants
* You cannot use `mut` with `const` 
* The type of the data for `const` variable must be annotated
* `const` variables can only be set to constant expressions, so number/string literals

# Strings
A `string slice` is an immutable view into another string. It is immutable by default, but it is possible to create a mutable slice with `mut`  
`string slice` can be created from a string literal or another object.  
To create a string slice:
```
let x: &str = "hello world!"; // A string slice from a string literal. This is how you create a string literal
let x = "hello world!"; // You can also  create a string literal like this.
let s: String = "hello world".to_string(); // A heap-allocated string
let s_slice: &str = &s; // A slice from the heap-allocated string
```

Strings in Rust are UTF-8 encoded.

# Arrays/Vectors
`Vector` is a dynamic array.  
A fixed-size array:
```
let four_ints: [i32; 4] = [1, 2, 3, 4];
         type---^    ^---count
```

A vector:
```
let mut vector: Vec<i32>> = vec![1, 2, 3, 4];
vector.push(5);

let vector2: Vec<i32> = Vec::new(); // We need the type annotation as we don't insert any values and compiler won't know what type we are storing
```

A slice can be created from a vector as well. Same as `string slice`, but for vector:
```
let slice: &[i32] = &vector;
```

You can compare vectors with `==` operator, the equality will be checked on an item by item basis so [1, 2] and [1, 2] vectors will be equal and [1, 2] and [2, 1] won't.  

# Tuples
A `tuple` is a fixed-size set of values of possibly different types:
```
let x: (i32, &str, f64) = (1, "hello", 34);
```

You can add a range of values into a tuple like and then easily turn it into a vector:
```
let t: Vec<i32> = (0..10).collect();
```

Destructuring a tuple:
```
let (a, b, c) = x;
```

Indexing a tuple. Indexing starts at 0:
```
let a = x.1;
```

# Generics
Generics on a type:
```
struct Foo<T> { bar: T }
```

Generics on an enum:
```
enum Optional<T> {
    SomeVal(T),
    NoVal,
}
```

Generics on a method:
```
impl<T> Foo<T> {
    fn bar(&self) -> &T {
        &self.bar
    }
}
```

# Traits
`Traits` are the equivalent of interfaces in C#   

Creating a trait:
```
trait Frobnicate<T> {
    fn frbnicate(self) -> Option<T>;
}
```

Using a trait:
```
impl<T> Frobnicate<T> for Foo<T> {
    fn frobnicate(self) -> Option<T> {
        Some(self.bar)
    }
}
```

# Pattern matching
`Pattern matching` is a similar concept to a switch...case   
To match a pattern, use `match` keyword:
```
let foo = OptionalI32::AnI32(1);
match foo {
    OptionalI32::AnI32(n) => println!("it's an i32: {}", n),
    OptionalI32::Nothing => println!("it's nothing!"),
}

struct FooBar { x: i32, y: OptionalI32 }
let bar = FooBar { x: 15, y: OptionalI32::AnI32(32) };

match bar {
    FooBar { x: 0, y: OptionalI32::AnI32(0) } => println!("The numbers are zero!"),
    FooBar { x: n, y: OptionalI32::AnI32(m) } if n == m => println!("The numbers are the same"),
    FooBar { x: n, y: OptionalI32::AnI32(m) } => println!("Different numbers: {} {}", n, m),
    FooBar { x: _, y: OptionalI32::Nothing } => println!("The second number is Nothing!"),
}
```

# Control flow
`For` loops:
```
let array = [1, 2, 3];
for i in array.iter() {
    println!("{}", 1);
}
```

`For range`:
```
for i in 0u32..10 { // Right exclusive range
    print!("{} ", i);
}
```
```
for i in 0u32..=10 { // Right inclusive range
    print!("{} ", i);
}
```

`If` statement:
```
if 1 == 1 {
    println!("Maths is working!");
} else {
    println!("Oh no...");
}
```

`If as expression`. The result of an if will be assigned to a variable:
```
let value = if true { // value will be equal to 'good'
    "good"
} else {
    "bad"
};
```

`While` loop:
```
while 1 == 1 {
    println!("The universse is operating normally.");
    break
}
```

`Infinite loop`:
```
loop {
    println!("Hello!");
    break // The only way to get out
}
```

`Loop as expression`. The result of the loop will be assigned to a variable. This is only possible with an infinite loop:
```
let mut counter = 0;
let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2; // Stop the loop and return a value
    }
};
```

# Functions
To create a function, use `fn` keyword. Argument types must be specified:
```
fn main(x: i32) {

}
```

To return a value from a function, use `->` followed by the type of the value:
```
fn test(x: i32) -> i32 {
    x + x
}
```

# Expressions
Examples of expressions:
```
let y = 6; // 6 is an expression 
let x = functionCall(); // function call is an expression
let vec = vec![1, 2, 3, 4]; // macro call is an expression
let y = {
    let x = 3;
    x + 1
}; // {} is an expression
```

# Memory safety & pointers
Each value has a variable that's called its `owner`.   
There can only be one owner at a time.  
When the owner goes out of scope, the value will be dropped.

Simple types are stored on the stack, complex types, like structs, are stored on the heap.  

In Rust, it is impossible for 2 variables to point to the same memory. When assigning one variable to another, the value of the first variable is copied into the second. This is called `shallow copy`. In addition to that, the first variable is invalidated and it no longer is an owner of the memory. So, in Rust, when copying one variable into another, it is called a `move`. This solves a problem where if we had 2 variables pointing to the same place in memory and they both went out of scope, we would have to free the memory, but since 2 variables point to it, one would be freed just fine, but the other would try to invalidated an already invalidated memory, which is a bug.Example:
```
let x = String::from("hello");
let y = x; // here x is moved into y and is invalidated

println!("{}, world!", x); // This will throw error as x doesn't hold any reference to the string hello anymore
```

To actually copy all the data, that is, do a `deep copy`, you would use `.clone()` method:
```
let s1 = String::from("hello");
let s2 = s1.clone(); // At this point, both s1 and s2 point to independent places in memory

println!("s1 = {}, s2 = {}", s1, s2); // This is fine now, s1 is still valid
```

Passing a variable to a function will move or copy, just as assignment does, sames rules apply. Example:
```
fn main() {
let s = String::from("hello");;

takes_ownership(s); // s is moved into a function
                    // s is not longer valid here, in this scope, it was moved into takes_ownership function and thus it is valid inside that function only

let x = 5;

makes_copy(x); // x is a simple type, therefore it is simply copied and can be used in the next line without any problems
}
```

Returning a variable from a function will move or copy.

If you want to pass a variable into a function with that function taking ownership of the variable, you use `references`. Example:
```
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // Passing variable by reference, it is not moved and can still be used in the scope of 'main'
    println!("Then length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // We got a reference to a string
    s.len()
}
```

Having a references as function parameters is called `borrowing`. You cannot modify a reference by default. You can declare a mutable reference with `&mut`, but with a few restrictions:
* the original variable must be mutable
* there can be no more than 1 mutable reference
* you cannot create a mutable reference when there's another immutable reference to the same variable

# Slices
A `string slice` is a reference to a part of the String:
```
let s = String::from("hello world");
let hello = &s[0..5]; // Slice that takes a range of characters from 0 to 5
let world = &s[6..11]; // Slice that takes a range of characters from 6 to 11
```

# Built-in functions/macros
`println!("{} {}", 1, 2);` - print something to the console with a new line  
`print!("{} {}", 1, 2);` - print something to the console without a new line
`vec![1, 2, 3, 4]` - create a vector   
`format!("{}-{}-{}", s1, s2, s3)` - format a string
`assert_eq!(2 + 2, 4)` - fail if something is not equal
`assert!(2 + 2 == 4)` - fail if something is not true
`ssert_ne!(2 + 2, 4)` - fail if something is equal
`panic!('Failed')` - crash program, kind of an exception throwing
`unimplemented!()` - panics with a message 'Not implemented'

# Modules
To define a module, use keyword `mod`:
```
mod foo {
    #[derive(Debug)]
    struct Foo {
        s: &'static str
    }
}
```

Use `pub` keyword to make something in a module public:
```
pub struct Foo {
    pub s: &'static str
}
```

You can put modules into their own files and use the module in another file with `mod` keyword:
```
mod foo;

fn main() {
    let f = foo::Foo::new("hello");
}
```

You can put modules into their own separate folders. The compiler will then look for MODNAME/mod.rs file:
```
mod boo; // boo is a module defined in its own folder, so mod.rs file has to exist in that folder

fn main() {

}
```

A module can contain other modules.

A keyword `use` can be used to define visibility of module names:
```
{
    use boo::bar; // Now you can simply write bar::...
    let q = bar::question();
}

{
    use boo::bar::question(); // Now you can simply write question
    let q = question();
}
```

To use another create, use keywords `extern crate`:
```
extern crate json;
```

To import a sibling module, you have to mod the module in main.rs:
```rust
// File src/a.rs
use super::b;
pub fn something() {
    b::something_else();
}

// File src/b.rs
pub fn something_else() {

}

// File src/main.rs
mod a;
mod b; // <---- This is needed for super::b to work in a.rs

fn main() {
    a::something();
}

```

# Testing
To make a function a test function, use `#[test]`.  
For unit tests, convention is to create a module named `tests` in each file with the code that they are testing and annotate it with `#[cfg(test)]`.  
Unit tests in Rust are able to test private functions without any problems.  
Integration tests are put in `tests` directory.  

You can't create integration tests for a binary crate, because you can't use the binary in some other file. However, when you create a binary crate, main calls the code in lib.rs and you absolute can run integration tests for lib.rs.

# Syntax
### Return statements
Return statements are possible if you want to return in the middle of the function. If a value is supposed to be returned at the end of the function, you can ommit `return` and a semicolon.  
Example:
```
fn add(x: i32, y: i32) -> i32 {
   // Some code here ... 
   x + y // implicit return statement
}
```

### Number literals
You can write numbers like this for easier reading: `100_000`.  

### Variable shadowing
Variables can be shadowed and it's not an error:
```
let x = 5;
let x = x + 1;
let x = x * 2;
println!("The value of x is: {}", x);
```

# Tips
Use `{:?}` to print something debug-style:
```
println!("{:?} {:?}", vector, slice);
```

Use tuples to return multiple values from a function:
```
fn calculate_length(s: String) -> (String, usize) {
    let length = s.len();

    (s, length)
}
```

Use `&s[start_index..end_index]` to extract part of the string. End index is not inclusive:
```
let s = String::from("hello world");
let hello = &s[0..5]; // Take characters from index 0 to index 5 - 1
let hello2 = &s[..5]; // Same as before, 0 can be omitted when we need to take from the start
let world = &s[6..11]; // Take characters from index 6 to index 11 - 1
let world2 = &s[6..]; // Same as before, 11 can be omitted when we need to take to the end
```

You can use `slices` on other collections too, like array or vector.

Rust has no ++ or -- operators, because the operators require knowledge of evaluation order and can lead to bugs.

Rust iterators have plenty of methods to do something we each item, like `fold`, `product`, `sum`, `map`, `filter`, etc. 

In a `match` statement, when doing a default `_` case, you can just write `()` if you don't want to do anything:
```
match c {
    'A': 65,
    'B': 66,
    _: () // Do nothing
};
```

To compare 2 chars to see if, for example, a char is n to z, you can use >:
```
if c > 'm' {
    // Do something
}
```

Use `_` in for range loops if you don't need an i:
```
for _ in 0..10 { // I only need the loop, but don't care about the index, so I write _ instead of i
    // Do something
}
```

Idiomatic rust: [https://cheats.rs/#idiomatic-rust](https://cheats.rs/#idiomatic-rust) and [https://github.com/mre/idiomatic-rust](https://github.com/mre/idiomatic-rust)

To parse a string into a number, use `str.parse::<i32>()`. Parse returns result enum, though it is possible to avoid match statement by using `unwrap` method

Sometimes `collect` method on the iterator can't infer the resulting type, so you can just write it like `collect::<String>` to tell the compiler to collect iterator into a string

In a `match` expression, if the expression is assigned to a variable, avoid having any side effects in the specific cases of the match.

`iter.next()` mutates the iterator, so the iterator must be mutable.

When a struct implements a method in which the struct is modified, it must get the first argument to the method should be `&mut self`.

To check if a number is a float, use `x == (x as u32) as f64`.

# Troubleshooting
### Issue: Cannot borrow iter as mutable
```
let iter = expr.chars().filter(|c| !c.is_whitespace());
let mut next = iter.next();
               ^ ERROR
```

Solution: `let iter = ...` should have been mutable, like `let mut iter = ...`, because iter.next() mutates the iterator.

### Issue: Want to move iterator into a function
Solution:
```
fn takes_iterator(iter: impl Iterator<Item = char>) {

}
```

### Issue: binary operation `!=` cannot be applied to type
Happens when you want to compare custom enum.  
Solution: make sure the custom enum derives from PartialEq, though keep in mind that enums contents also implement PartialEq

### Issue: `print!` doesn't print anything before a read line
The following code would only print `>>` after the input has been read:
```
print!(">>");
io::stdin().read_line().unwrap();
```

Solution:
```
print!(">>");
io::stdout().flush().unwrap();
...
```
`print!` macro doesn't print anything directly to the screen, but rather to a buffer in memory and then when the buffer is flushed, things appear on the screen. So we have to manually flush the buffer.