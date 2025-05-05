---
publish: "true"
date: 2025-05-05
---
##### Library crates vs. binary crates
- A library crate is a crate that other people can pull into their projects as a dependency
	- We can create one using a command like `cargo new communicator --lib`, where 'communicator' is the name of our crate
		- Notice how we pass `--lib` instead of `--bin` at the end of our command
	- This creates a file `src/lib.rs` instead of `src/main.rs` that contains an example test
##### The module filesystem
- We can define a module in Rust with the following syntax:
```rust
mod network {
	fn connect() {
	}
	mod client {
		fn connect() {
		}
	}
}
```
- Here, we define 2 modules: 'network' and 'client'
	- If we want to call the 'connect' functions in this code outside of the network module, we need to use the `::` operator
	- Notice that even though the two functions are named the same, they're namespaced differently - as `network::connect` and `network::client::connect`
		- Thus they can have completely independent implementations and don't conflict!
		- This is analogous to file systems. You can think of 'client' as a directory within 'network' (its parent directory)
- Now let's say we want to define a third module, 'server', but want to initialize it somewhere else to avoid cluttering this file
	- Then we can declare `mod server;` at the top of our file and then create a file `server.rs` in the root directory that contains our module definition
		- This is basically telling the compiler to look for a file `src/server.rs` and reference it for all function definitions from this module
		- By default Rust only knows about `src/lib.rs`, and we need to declare other files that we want to bring into scope
	- Be careful not to surround your code in `server.rs` with `mod server {` - this is implicit!
- Now let's say we have a submodule defined in `server.rs` with the following contents:
```rust
mod auth {
	fn login() {
	}
}
```
- And let's say that our 'login' function is super long and we wanted to cut down on the clutter
	- Simply declaring `mod auth;` and defining auth in `src/auth.rs` would trigger a compiler error, because we're declaring this in a submodule
	- Thus we need to move the content of `server.rs` into `server/mod.rs` and then move our 'login' function into `server/auth.rs`
	- Now, we can proceed with initializing `mod auth;` in `server/mod.rs`!
- In summary:
	- If a module has no submodules, then you can put its declarations in `[name].rs` file
	- If a module does have submodules, then place its contents in `[name]/mod.rs` and its submodule(s) in `[name]/[submodule_name].rs`
##### Controlling visibility with `pub`
- So, why does the compiler warn us that the function declaration `auth::login` is going unused? Isn't this technically misleading, since libraries are supposed to be used by *users*/other programs?
	- No! This is because the default state of all code is private, which means that the function `auth::login` is private and if we don't call it within our library code, no other file has rights to use it, even if it's called in via `extern crate communicator` and `communicator::network::server::auth::login();`
	- When a function is marked as public (which must be done explicitly), Rust assumes that it'll be called outside the current module and those compiler warnings go away
- We can declare a function as public by adding the `pub` keyword before its declaration
	- But to make `auth::login` public, we need to make both 'auth' and the function 'login' public, as follows:
```rust
// [src/network/mod.rs]
pub mod auth;

// [src/network/auth.rs]
pub fn login() {
}
```
- General rules for item visibility:
	- If an item is public, it can be accessed through any of its parent modules
	- If an item is private, it can be accessed only by its immediate parent module and any of the parent's child modules
##### The `use` keyword, `glob` syntax, and `super`
- Let's say we have a function nested within several modules, that we need to access like `a::series::of::nested_modules()`
	- This can get quite lengthy, so we take advantage of Rust's `use` keyword to bring part of the path into scope
- Let's say for example that we're using a bunch of functions defined in the 'of' module and don't want to specify this lengthy path over and over again; we can write the following:
```rust
use a::series::of;

fn main() {
	of::nested_modules();
}
```
- Notice that all of the children of modules aren't brought into scope - just the modules themselves (this is why we still need to specify 'of' in `of::nested_modules()`)
- Since enums form a namespace as well, we can do a similar thing for them!
```rust
enum TrafficLight {
	Red,
	Yellow,
	Green,
}

use TrafficLight::{Red, Yellow};

fn main() {
	let red = Red;
	let yellow = Yellow;
	let green = TrafficLight::Green;
}
```
- If we want to pull in all items in a namespace at once, we can use the `*` syntax, which is called the glob operator. Below is a refactoring of the code we just wrote using this syntax:
```rust
use TrafficLight::*;

fn main() {
	let green = Green;
}
```
- Use globs sparingly, because they usually bring more than what you need into context and sometimes cause naming conflicts
- Let's take the following code snippet:
```rust
pub mod client;

#[cfg(test)]
mod tests {
	fn call_connect() {
		client::connect();
	}
}
```
- Our call within `call_connect()` would result in a compiler error, because technically `client::connect` belongs to the root (or in this case, parent) module and we are within a submodule `test` when we're calling it
- There are two ways to fix this:
	- `::client::connect()`, where the `::` prefix means that we need to specify the full-length path from root
		- Usually not preferred because it's quite lengthy in most cases
	- `super::client::connect()`, where the `super::` prefix moves us up one level and allows us to call any sibling modules (which `client` qualifies as)
		- It's annoying to type `super::` before every declaration, so we usually just include something like `use super::client;` at the top of our module
> [!note]- References
> - Chapter 7 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.