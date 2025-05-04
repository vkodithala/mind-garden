---
publish: "true"
date: 2025-05-04
---
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
> [!note]- References
> - Chapter 4 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.