---
publish: "false"
---

- Why Rust? It's important to answer this question to motivate our learning
	- Systems/low-level programming offers benefits like faster runtimes, but it's incredibly hard to work with because of subtle bugs that come from things like memory-safety issues
	- Rust avoids these bugs with a 'smart' compiler that refuses to compile code with elusive bugs
		- This allows devs to focus on program logic rather than chasing down bugs
- Rust also brings contemporary developer tools to the systems programming world:
	- Cargo: a dependency manager that makes adding, compiling, and managing dependencies seamless
	- Rustfmt: a linter that ensures consistent coding style
	- Rust language server: a language server that powers code completion/inline error messages on IDEs
- In a nutshell, Rust just makes low-level programming much more straightforward and approachable
	- This is why it has such a big fanbase
- Rust is an *ahead-of-time compiled* language, which means that you can make an executable (via something like `rustc main.rs`) and give it to someone else. They can then run it without having Rust itself installed
	- Other languages (like Python) don't require compile-run cycles and instead have only 1 command, but require language support for someone to run programs
	- Trade-offs!
- Three main commands to know when working with Cargo, Rust's built-in dependency manager
	- `cargo build` creates an executable file in the `target/debug` subdirectory
		- `cargo build --release` is what you should run when you're building for release, not development
			- Benefit is that it adds optimizations to make your executable run faster, but these take more time to apply (so not optimal for quick testing)
			- Places executable in `target/release` instead of `target/debug`
	- `cargo run` handles building and running all in 1 command, so you don't have to find and run the executable file
	- `cargo check` just checks that there aren't any compilation errors -- we can call this periodically as we're coding to check ourselves
#### Chapter 3: Common programming concepts
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
#### Chapter 4: Understanding ownership
- Managing data on the heap is the reason why ownership exists
	- When you put data on the heap, you ask for a fixed amount of space, and the OS finds a spot for your data (this is the abstraction that `malloc` supports in C)
	- Ownership involves keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused pointers
	- We can alternatively use the stack as memory at runtime, but the stack stores and removes data in a FIFO fashion such that you don't need to specify how much you want stored (works deterministically)
		- This is typically used to save/pop function arguments
- We've already seen string literals, where string values are hardcoded into the program
	- This standard data type stores data on the stack -- let's look at an example where data is stored on the heap
	- The `String` datatype allows us to do this
```rust
let s = String::from("hello");
s.push_str(", world!");
println!("{}", s);
```
- When we call `String::from`, the data type's implementation requests the memory that it needs from the heap
	- But garbage collection is where this becomes difficult (for mutable string types): we need to pair exactly one `alloc` with exactly one `free`
	- When a variable goes out of scope (`s`, for example), Rust calls a special function called `drop` that frees/returns the memory
		- Called automatically at the closing curly brackets
- Copying datatypes like `&str` that are stored on the stack is relatively inexpensive, so they are by default deep-copied
	- For types like integers, booleans, characters, and tuples that are stored on the stack, Rust defines a trait `Copy` that makes older variables usable after re-assignment, and deep copies data to aliases
	- But when we copy data that's stored on the stack without using `clone`, Rust simply moves the ownership to the second variable and invalidates the first
```rust
let s1 = String::from("hello");
let s2 = s1;
```
- With `s1` being invalidated, and `s2` pointing to data on the stack that holds the same metadata directing us to a heap allocation
- Deep-copying heap data is done by using `clone`, as shown below (this overrides the way that Rust handles this automatically, and may lead to longer runtimes)
```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```
- When we use a variable as an argument to a different function and it doesn't have the `Copy` trait, it's moved to that second function and thus can't be used afterwards
	- Similarly, returning a value transfers ownership to the function that we're returning to
- "Anything we need to pass in needs to be passed back if we want to use it again" -- but this seems tedious, how do we overwrite it?
	- To avoid always taking ownership of the value, we can use pass-by-references and pass in references instead (see below)
```rust
fn main() {
	let s1 = String::from("hello");
	let len = calculate_length(&s1);
	
	println!("length is: {}", len);
}

fn calculate_length(s: &String) -> usize {
	s.len();
}
```
- References that are borrowed are immutable by default; we can make mutable references as follows:
```rust
let mut s = String::from("hello");
{
	let r1 = &mut s;
}
```
- Here, we need to make *mutable* references and pass them into consuming functions
	- Two caveats:
		- (1) Only 1 mutable reference can exist in a single scope -- this is to prevent data races which can occur without proper synchronization
		- (2) Mutable and immutable references cannot coexist in a scope -- this is because consumers of immutable references think the data they're consuming is static, and doing this would break that abstraction
- Rust prevents us from returning dangling references, which are references to data that has been de-allocated (see below)
```rust
fn dangle() -> &String {
	let s = String::from("hello");
	&s
}
```
- After the curly braces here, `s` will be de-allocated, but we're returning a pointer to it to the calling function, which is a problem
	- Rust prevents this at compile-time
- A string slice is a reference to a part of a `String`, and is denoted by `&s[a..b]`, where `a` is the first position in the slice and `b` is the last position plus 1
- This helps us understand why string literals are immutable
	- When we have something like `let s = "Hello, world!";`, the type of `s` is `&str`, which means that it's effectively a slice pointing to the entire string's binary
- We can improve the signature of our previous function `first_word` by having it take in `&str` such that we can pass in both `String` (via slices) and `&str`
```rust
fn main() {
	let my_string = String::from("hello world");
	let word = first_word(&my_string[..]);

	let my_string_lit = "hello world";
	let word = first_word(&my_string_lit[..]);
	// also works, b/c literals *are* slices
	let word = first_word(my_string_lit);
}

fn first_word(s: &str) -> &str {
```
#### Chapter 5: Using structs to structure related data
- Structs are custom data types that let you name/package together multiple related values that make up a meaningful group
	- Analogous to an object's data attributes in OOP
```rust
struct User {
	username: String,
	email: String,
	sign_in_count: u64,
}

fn build_user(email: String, username: String) -> User {
	let user1 = User {
		email,
		username,
		active: true,
		sign_in_count: 1
	}
	let user2 = User{
		email: String::from("krishna.verma@gmail.com"),
		...user1
	}
}
```
- Bottom-most code shows the struct update syntax, which allows us to create new struct instances that only update part of the code (basically just a shortcut)
	- `user1` code shows the *field init shorthand*, which is particularly useful when some of our fields are parameters
	- Either the entire instance must be mutable, or none of it (initialized with `let mut user1` or `let user1`)
	- We want instances of our structs to own all of their data, so we use datatypes like `String` vs. references like `&str`
		- In Chapter 10, we discuss how to handle situations where we *do* want to use references using lifetimes
- Tuple structs are a construct that lets us create structs from tuples such that we can differentiate between them by name
	- Initialized like `struct Color(i32, i32, i32)`
	- Can access elements with a `.` followed by the index
- We can't print out our struct by default because Rust doesn't really know how it should present them, so we use the `#[derive(Debug)]` syntax, as shown below:
```rust
#[derive(Debug)]
struct Rectangle {
	width: u32,
	height: u32,
}

fn main() {
	let rect1 = Rectangle { width: 30, height: 50 };
	println!("rect1 is {:?}", rect1);
}
```
- We can use `{:#?}` instead of `{:?}` for larger structs that we want presented as multi-line outputs
- Now we introduce an OOP-like way for us to specify functions that can be performed on a certain class
```rust
impl Rectangle {
	 fn area(&self) -> u32 {
		 self.width * self.height
	 }
}
```
- Here, `impl Rectangle` indicates that all of the functions that follow are implemented on the `Rectangle` struct
	- Then, in those functions, we can pass in either `&self, &mut self, self`, the latter of which we wouldn't really use unless we're transforming it into a different object and want to prevent the caller from using the original instance after the fact
- For functions, Rust implements automatic referencing/dereferencing such that the method is called properly -- we already know which of `&self, &mut self, self` it takes in, so we can reference/dereference it ourselves
	- Thus `p1.distance(&p2)` and `(&p1).distance(&p2)` both evaluate to the same thing in practice
	- This makes Rust ergonomic!
- Associated functions are functions within `impl` blocks that *don't* take `self` as a parameter
	- Generally used as constructors that return new instances of the struct
	- To call associated functions, we use `::` syntax, because they are name-spaced by the struct (we'll discuss name-spacing/modules more in Chapter 7)
#### Chapter 6: Enums and pattern matching
##### Using structs
- One situation where we might want to use enums is when we're working with IP addresses
	- We know that any IP address is one of two things: a v4 or v6 address, and it cannot be both at the same time
	- Enum values can only be 1 of some # of variants, so it's perfect here
- We define the enumeration and its "variants" (types/fields) as follows
```rust
enum IpAddrKind {
	V4,
	V6,
}
```
- This is now a custom data type that we can use elsewhere in our code
- The variants of an enum are namespaced under its identifier, so we can access/use them like `IpAddrKind::V4` or `IpAddrKind::V6`
- Let's say that we wanted to store the addresses themselves along with the variants
	- Rust provides us with a way to do this, also
```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
```
- And a useful thing is that we can store these addresses as different data types within enums
	- This isn't possible with vanilla structs!
- Just as we're able to define methods on structs, we can define them also on enums, using the same `impl {ENUM_NAME}` syntax
	- And then we call these functions with the dot operator
##### Expressing nullity in Rust
- The null operator is error-prone in implementation, so Rust decides to exclude it from the language altogether
	- In its place, it defines an enum `Option` that encodes the concept of a value being present or absent
```rust
enum Option<T> {
	Some(T),
	None,
}
```
- Some examples of using `Option` values to hold different types
	```rust
let some_string = Some("a string");
let absent_number: Option<i32> = None;
```
	- Notice that when we used `None`, we had to tell Rust what type of `Option<T>` we have because type inference is effectively impossible (there's nothing to base it on)
- So how is this useful to us? The problem with the null operator is operations involving it often have non-deterministic results when using possibly null values in functions
	- In Rust, we can't use the `Option<T>` items within calculations with non-Options as it results in type mismatches
	- Thus the compiler makes sure that we handle the null case before using a value with type `Option` - this is done by converting an item of type `Option<T>` to one of type `T` explicitly before using it
- How do we do this? `match` expressions
	- `match` expressions in Rust are analogous to switch statements in languages like C, and allow us to return different output for different enum values
	- We call these handlers "arms", and a manifestation of these expressions is shown below
```rust
fn value_in_cents(coin: Coin) -> u32 {
	match coin {
		Coin::Penny => {
			println("Lucky penny!");
			1
		},
		Coin::Nickel => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25,
	}
}
```
- When we store inner values of data within enums, we can actually use these within `match` expressions also. Continuing with the previous example, we have `Coin::Quarter(state) => 25` and in the enum, `Quarter(UsState)` where `UsState` is another enum
- One very common construct in Rust is using `match` statements to perform operations on values of type `Option<T>` - see below
```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i + 1),
	}
}
```
- Pattern here: match against an enum, bind a variable to the data inside, and execute code based on it
- Rust's `match` clauses must be exhaustive. In other words, all cases must be considered or our compiler is mad at us!
- However this can get tedious when we have a ton of possibilities (such as a numerical range), so we can define `_ => (),` as our last arm and this means that all enum values that aren't explicitly mapped have no effect
- But even this can get wordy when we only care about one case. Introducing `if let`
- 