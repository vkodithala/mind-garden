---
publish: "true"
date: 2025-05-04
---

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
##### `match` expressions
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
##### Introducing `if let`
- But even this can get wordy when we only care about one case. Introducing `if let`
- Let's say that we want to count all non-quarter coins while also announcing the state of the quarters that we see. We can write the following with our `match` control flow:
```rust
let mut count = 0;
match coin {
	Coin::Quarter(state) => println!("State quarter from {:?}!", state),
	_ => count += 1,
}
```
- But this seems a little too verbose since we're only checking for one pattern
- Instead, we can use an `if let` flow, which uses semantics that we're familiar with as programmers
```rust
let mut count = 0;
if let Coin::Quarter(state) == coin {
	println!("State quarter from {:?}!", state);
} else {
	count += 1;
}
```
- The main trade-off here is that match statements are always exhaustive (else our compiler won't let us proceed), whereas `if let` leaves a lot of discretion to the programmer and is a little less safe
> [!note]- References
> - Chapter 6 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.