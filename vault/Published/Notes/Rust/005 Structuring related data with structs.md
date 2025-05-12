---
publish: "true"
date: 2025-05-04
---
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
> [!note]- References
> - Chapter 5 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.