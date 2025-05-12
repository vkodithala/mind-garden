---
publish: "true"
date: 2025-05-11
---
##### Removing duplication by extracting a function
- Let's say that we wanted to find the largest number in a vector of unsigned integers, and wrote the following code to do so:
```rust
let number_list = vec![34, 50, 25];
let mut largest = number_list[0];

for number in number_list {
	if number > largest {
		largest = number;
	}
}

number
```
- If we wanted to find the largest number in a different vector, then we'd need to duplicate this code
- Instead, we can extract a function that takes in a reference to an array of unsigned integers `&[i32]` and returns the largest one
	- Here's an example:
```rust
fn find_largest(arr: &[i32]) {
	let mut largest = arr[0];
	
	for &num in arr.iter() {
		if num > largest {
			largest = num;
		}
	}
	
	largest
}
```
- And then we can simply call `find_largest` on the two arrays that we want to extract the largest number from
- Now, let's say that we want to create a function that's capable of returning the largest element of any array (could be an array of unsigned integers, or characters, etc.)
	- We can do so using generics:
```rust
fn largest<T>(list: &[T]) -> T {
	let mut largest = list[0];

	for &num in list.iter() {
		if num > largest { // compiler error
			largest = num;
		}
	}

	largest
}
```
- We do, however, get a compiler error when we use the `>` operator
	- This is because some types don't implement the trait `std::cmp::PartialOrd` (since we haven't yet discussed traits, it's sufficient to say that some types aren't ordered and thus can't be compared)
##### Using generics with structs and enums
- We can also define structs with generics, and implement methods on their instances with either generic or specific types
```rust
// defines a Point type, with 2 values of the same type
struct Point<T> {
	x: T,
	y: T,
}

// in this struct def, the coordinates can have different types (i.e. i32, float)
struct Point<T, U> {
	x: T,
	y: U,
}

// impl methods on instances w/ generic types
impl<T> Point<T> {
	// returns ref to data in field 'x'
	fn x(&self) -> &T {
		&self.x
	}
}

// impl methods on instances w/ specific types
impl Point<f32> {
	fn distance_from_origin(&self) -> f32 {
		(self.x.powi(2) + self.y.powi(2)).sqrt()
	}
}
```
- In the above code, instances of `Point<T>` where T isn't of type `f32` won't have the `distance_from_origin` method defined
	- This is b/c we use operations that are only defined for floating point types and are not broadly generalizable
- Enums can also be defined similarly with generics. We will use the `Option` and `Result` types that we learned about in [[006 Enums and pattern matching]] and [[009 Error Handling]] as examples:
```rust
enum Option<T> {
	Some(T)
	None, // has no type
}

enum Result<T, E> {
	Ok(T),
	Err(E),
}
```
- Rust's generics are extremely efficient at runtime due to a process called *monomorphization*
	- Basically, Rust sees where we call generics and replaces generic definitions with specific ones at compile time
	- All of the cost is incurred at compile- (and not run-) time!
##### Traits: defining shared behavior
- A trait tells the compiler about specific functionality that a type has and can share with other types
	- This is akin to an interface in object-oriented languages like Java
- A type's behavior = the methods that we can call on that type
	- We define them in a trait as follows:
	```rust
pub trait Summary {
	fn summarize(&self) -> String;
}
```
- Here, we define a trait `Summary` and in its definition, we include method signatures followed by semicolons
	- Any type that has this trait must also define the function `summarize` with a return value type of `String`
- Let's say that we have 2 structs `NewsArticle` and `Tweet` that we want to have the `Summary` trait so that we can call `.summarize()` on them. Then we can *implement* our trait as follows:
```rust
impl Summary for NewsArticle {
	// same as declaration from earlier, except followed with an implementation
	fn summarize(&self) -> String {
		format!("{}, by {}", self.headline, self.author)
	}
}

impl Summary for Tweet {
	fn summarize(&self) -> String {
		format!("{}: {}", self.author, self.content)
	}
}
```
- As seen in this code snippet, trait implementations include the `impl` keyword followed by the trait name that we want to implement and the type we want to implement it for
- It seems like two crates could implement the same trait for the same type, but Rust prevents this with restrictions that follow the property of *coherence*
	- The only case in which we're allowed to provide trait implementations is if the trait itself is local to our crate, or the type is
	- In other words, we cannot implement external traits on external types
- We can also provide default implementations for trait methods, which will be used in lieu of a concrete implementation in type implementations
	- For instance, see the following example of a trait with a default implementation and its implementing type
```rust
pub trait Summary {
	fn summarize_author(&self) -> String;
	fn summarize(&self) -> String {
		format!("Read more from {}", self.summarize_author())
	}
}

impl Summary for Tweet {
	// we are required only to implement `summarize_author`, and can optionally overwrite the implementation of `summarize`
	fn summarize_author(&self) -> String {
		format!("@{}", self.author)
	}
}
```
##### Trait bounds
- When we use a function like `summarize` that requires a concrete implementation of a trait, we sometimes want to make sure that our argument types (when using generics) implement the same trait
	- For this, we can use something called *trait bounds*
- Trait bounds basically ensure that the types of our arguments implement specific traits
- There are two ways with which we can denote trait bounds:
```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 {}

fn some_function<T, U>(t: T, u: U) -> i32
	where T: Display + Clone,
	      U: Clone + Debug {}
```
- These are both equivalent ways of using trait bounds
	- The first is more verbose and better for single generic arguments/arguments that only require one trait implementation rather than multiple chained together
	- The second uses the `where` clause to make trait bounds with more arguments/constraints easier to read
- We can also use trait bounds to conditionally implement methods for types that implement specific traits
	- Say for example that we have a generic struct `Pair<T>` that has a setter `new` by default, but we wanted to implement a function `cmp_display` if the type used is able to be compared and printed out (in other words, implements the `Display` and `PartialOrd` traits)
	- We can do this like so:
```rust
// methods implemented for Pair<T> where the type implements traits Display and PartialOrd!
impl<T: Display + PartialOrd> Pair<T> {
	fn cmp_display(&self) {
		if self.x >= self.y {
			println!("The largest number is x = {}", self.x);
		} else {
			println!("The largest number is y = {}", self.y);
		}
	}
}
```
- Let's say that we want to ensure that some trait is implemented for any type that implements another trait. A concrete example comes from the Rust standard library, where the `ToString` trait is implemented for any item that has type `Display`
	- This is to ensure that we can call `.to_string()` on any element that implements the `Display` trait, which is a key quality of the trait itself
- In the standard library, we do so as follows (and this same framework can be used for other 'blanket implementations'):
```rust
// implementation is processed for any generic T that implements the `Display` trait
impl<T: Display> ToString for T {}
```
##### An introduction to lifetimes
- The main purpose of lifetimes is to prevent dangling references, which is where we access data that may be outdated/inaccurate
- Take for instance the following code:
```rust
let r;
{
	let x = 5;
	r = &x;
}
println!("r: {}", r);
```
- After the last curly brace in the above code, the variable `x` goes *out of scope*, and is thus de-referenced and cleaned up
	- But recall that we set the value of `r` to this de-referenced variable, and thus this is an invalid/dangling reference
	- Our compiler complains!
- Let's say that we have the following code that returns a reference to the longest string given two string slices:
```rust
fn longest(x: &str, y: &str) -> &str {
	if x.len() > y.len() {
		x
	} else {
		y
	}
}
```
- This code won't compile, but why?
	- Since `x` and `y` are inputs from outside of the function, Rust doesn't know when they'll be dropped/how long they'll be valid for
- Take the following code, which is problematic:
```rust
fn main() {
	let string1 = String::from("return of the jedi");
	let result;
	{
		let string2 = String::from("revenge of the sith");
		result = longest(string1.as_str(), string2.as_str());
	} // string2 is dropped here, and result -> dropped ref
	println!("the longest star wars movie title is: {}", result);
}
```
- If Rust were to compile our `longest` function as-is, it wouldn't realize that `result` is pointing to a dangling reference after `string2` is dropped in the inner curly-braces
	- To prevent this, Rust triggers a compilation error when we don't specify what are called *generic lifetimes* in functions that return references
- So we modify our `longest` function to look like this:
```rust
fn longest(x: &'a str, y: &'a str) -> &'a str {
	...
}
```
- Here, `'a` is a lifetime parameter - we could've named this any word, followed by an apostrophe, but people generally like to use `'a` or other characters/short words
	- What this is saying is that the input references and return reference all share a generic lifetime
	- In other words, the generic lifetime `'a` will get the concrete lifetime that's equal to the smaller of the lifetimes of our inputs
- Now, when the lifetime of the reference to `y` goes out of scope, Rust knows that `result` is mapped to a dangling reference, and can notify us of this using compile errors
- So far, we've assumed that structs only hold owned types
	- But it's possible for them to also hold borrowed types/references, but we must add lifetime annotations to these structs so that Rust knows when they have dangling references
- Take the following code:
```rust
struct JediLocation<'a> {
	planet: &'a str,
}

fn main() {
	 let planet = "Ahch-To".to_string();
	 let planet_ref: &str = planet.as_str();
	 let skywalker_location = JediLocation {
		 planet: planet_ref,
	 }
}
```
- Now, our compiler knows that as soon as `planet_ref` goes out of scope, the struct `skywalker_location` does as well (any of its uses after this point would be discarded)
	- Thankfully this isn't something that we have to worry about here, since the lifetimes of the struct and its inner reference are the same
- After writing a bunch of Rust code, the Rust team found out that there were patterns where developers wrote out (often needless) lifetime annotation
	- To cut down on this unnecessary annotation, they introduced the lifetime *elision rules*, which are 3 rules that indicate when Rust's compiler is able to infer lifetimes in functions that use references
	- Here are the three rules:
		- **Rule I.** Every input parameter that's a reference gets its own lifetime parameter. i.e., a function with 1 parameter gets one lifetime parameter, a function with 2 gets two lifetime parameters, and so on
		- **Rule II.** If there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters
			- In the example below, `fn first_word(s: &str) -> &str` is compiled to `fn first_word(s: &'a str) -> &'a str` based on this rule
		- **Rule III.** If there are multiple parameters but one of them is `self`, the lifetime of `self` is assigned to all output parameters
			- This makes it so we don't have to annotate lifetimes on method signatures that often
- Let's take the following function from earlier, which used references but didn't require a lifetime annotation:
```rust
fn first_word(s: &str) -> &str {
	let bytes = s.as_bytes();

	for (i, &item) in bytes.iter().enumerate() {
		if item == b' ' {
			return &s[0..i];
		}
	}
	
	&s[..]
}
```
- This should make sense to you now based on the 3 rules that we introduced earlier (specifically Rule II.)
	- Expanding more on Rule II: when we have methods in a struct implementation that use `&self` or any other reference, the methods themselves don't need lifetime annotations, but the `impl` header does
- Here's an example:
```rust
impl<'a> ImportantExcerpt<'a> { // self now has the lifetime 'a
	fn announce_and_return_part(&self, announcement: &str) -> &str {
		println!("announcement from your emperor: {}", announcement);
		self.part // this return param is valid for 'a (same as self)
	}
}
```
- One special lifetime that we can specify is `'static`, which denotes that a reference is valid for the entire duration of a program
	- For example, string literals have the `'static` annotation by default because they're stored directly in the binary of our program (vs. the heap), so live until the program itself dies
	- Our compiler will often suggest us to use the `'static` annotation to fix dangling references, but this is often not the right thing to do! Consider first if you want to make a reference accessible for the whole duration of a program
> [!note]- References
> - Chapter 10 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.