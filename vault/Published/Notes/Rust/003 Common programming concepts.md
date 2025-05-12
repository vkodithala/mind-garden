---
publish: "true"
date: 2025-05-04
---
- By default, variables in Rust are immutable -- so if we write something like `let x = 5;` and then later on attempt to update the value of x, we'll get a compiler error
- But we're provided with an interface to make variables mutable
	- We simply need to declare them with the `mut` keyword, like `let mut x = 5;`
- There are some trade-offs here
	- Leaving variables immutable increases clarity during updates, in a functional programming style
		- But this is only suitable for smaller data structures that don't require a ton of complexity to copy over
	- Editing in-place (mutability) makes more sense for larger data structures that incur a penalty both in complexity and runtime from copying over
- Constants are variables that we can't use the `mut` keyword on and remain immutable for their entire lifespan
	- Another property of constants is that they can't be set to the result of functions and take constant numerical values
	- Must also be statically typed - type inference doesn't work with constants
	- An example is:
```rust
const GLOB_VAR: u32 = 100_000;
```
- Constants can be locally or globally scoped, and the convention in writing them is shown above
- Shadowing is another useful idea in Rust. By declaring `let` on a variable that's previously been defined, we re-set its value to something else
	- This could be a transformation of the original value, or something new entirely
	- Datatypes of old and new values *don't* have to match - which makes this different than simply using `mut`
- Rust is a *statically typed* language, which means that it must know the types of all variables at compile-time
	- But the compiler usually does a pretty good job at type inference
	- An example of a situation where we'd need to specify the type is as follows:
```rust
let guess: u32 = "42".parse().expect("Not a number!");
```
- Default integer type is `u32`, default floating point number type is `f64` (relatively same performance as `f32` with higher precision)
	- We'll seldom deviate from this
- Rust has two primitive compound types: tuples and arrays
	- We can declare and de-structure tuples as follows
```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
println!("The value of y is: {}", y);
println!("The value of y is: {}", tup.1);
```
- Arrays are the other compound data type which differ from tuples in that their objects can only be of 1 type
	- Also, arrays can't shrink/grow like they can in most other languages (we use vectors for this)
	- When you index through arrays with something like `arr[1]`(Pythonic construct), Rust actually does compile-time checks to prevent out-of-bounds accesses
		- These are a huge safety issue in C, and Rust prevents us from doing this
- Rust doesn't care *where* you've defined functions, only that you've defined them somewhere in the file
- Function arguments *must* be typed, but obviously don't all have to be of the same type
- We must also distinguish between statements and expressions; the following example is useful
```rust
let y = {
	let x = 3;
	x + 1
};
```
- Here, `y` actually evaluates to 4. This is because `x + 1` is an expression, and returns a result (in this case, 4)
	- Expressions don't end in semicolons; if we add a semicolon, they become statements
- By contrast, doing something like `let y = (let x = 3;)` is illegal, because the latter doesn't actually *return* anything
- By default, a function's return value is within its last line (unless we return early)
	- We can specify the return type using the `->` keyword after the `()` in a function declaration, like `fn five() -> u32`
- Unlike Python, Rust requires conditional variables to actually have a `bool`variable before proceeding with control flow; the following code would produce an error
```rust
let number = 3;
if number {
	println!("A number exists!");
}
```
- Rust doesn't automatically try to convert non-boolean types to a boolean (like other languages: JS, Ruby, Python) do
- We can also use ifs within our let statements, as follows (this is Rust's way of supporting a ternary, it seems):
```rust
let number = if condition {
	5
} else {
	6
};
```
- BUT the variables on each side of the if-else clause here must both have the same type, because variables themselves can only have 1 type
	- This is to allow Rust to verify at compile time what the type of the variable is
- Rust supports `for` and `while` loops, but the former is written a little differently -- still operates based on a range, like Python, but this is written differently
```rust
a = [10, 20];
for element in a.iter() {
	println!("the value is: {}", element);
}
```
> [!note]- References
> - Chapter 3 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.