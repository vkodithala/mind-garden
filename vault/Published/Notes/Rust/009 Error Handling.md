---
publish: "true"
date: 2025-05-10
---
- Rust divides errors into two types: recoverable and unrecoverable
	- Recoverable errors are those where it's reasonable to report to the user and get them to retry the operation (i.e. a FileNotFound error)
		- These return the type `Result<T, E>`
	- Unrecoverable errors are largely outside of a user's control and are the result of program bugs, like an out-of-bounds access in an array
		- These have the type `panic!` and cause the program to halt execution
##### Unrecoverable errors with `panic!`
- `panic!` is a function that's explicitly called by library functions (and can in fact be called by any developer in their code) to notify us of software bugs
	- By default when a panic occurs, Rust walks back up the stack and cleans up data from each function it encounters. This is obviously compute-intensive and may slow down our program, so we can edit our `Cargo.toml` file to turn *abort* mode on and prevent the stack cleanup
- As developers, we want to figure out what's causing our code to panic. We can do so with a backtrace
	- We can run the command `RUST_BACKTRACE=1 cargo run`, which gives us the 'history' of where our problem originated, starting with the most direct culprit
	- We want to scroll down this history until we see files that we've created/edited, and further inspect those files to figure out what's going on
##### Recoverable errors with `Result`
- The `Result` enum is defined as having two variants: `Ok(T)`, where T represents the type of value that'll be returned in the success case, and `Err(E)`, where E represents the type of error that'll be sent back otherwise
```rust
enum Result<T, E> {
	Ok(T),
	Err(E),
}
```
- Certain functions (like `File::open`) need to tell us whether they succeeded or failed and at the same time deliver us return values or error codes
	- The `Result` enum's purpose is to convey this information. Under the hood, `File::open` does actually return a `Result` object, which we have to handle (likely with a `match` or `if let`)
- Here's an example of how we can handle a return value of `Result`type with a match statement:
```rust
use std::fs::File; // Result enum/variants imported before prelude, so can call 'Ok' and 'Err' without any extra work
use std::io::ErrorKind

fn main() {
	let f = File::open("hello.txt");

	let f = match f {
		Ok(file) => file,
		Err(ref error) => if error.kind() == ErrorKind::NotFound => {
			match File::create("hello.txt") {
				Ok(fc) => fc,
				Err(e) => {
					panic!("Tried to create but problem! {:?}", e)
				},
			}
		},
		Err(error) => {
			panic!(
				"There was a problem opening the file: {:?}",
				error
			)
		}
	}
}
```
- In the code above, the second arm of our `match` expression includes what's called a *match guard*
	- This is an extra condition on a match arm such that we only process the code in the arm if it's true
	- `ErrorKind` has several types which indicate the errors that can come about in Rust; we must import these explicitly to use them
	- And we pass in a `ref` object to our match guard such that it doesn't take ownership, and use `error.kind()` to extract the type of error
- Ultimately, match expressions are verbose and we don't always need them
	- So we introduce another syntax for error handling, which is useful in the case that we want to panic when an `Err(E)` type is returned
	- The `.unwrap()` operator allows us to set a variable to the result if `Ok`, else calls `panic!` with a pre-defined error message
	- The `.expect(error_msg)` operator does the same thing, except returns `error_msg` as the message in the panic
		- This is oftentimes useful for tracking where our errors originated from
```rust
use std::fs::File;

fn main() {
	// using unwrap
	let f = File::open("the_code.rs").unwrap();
	// using expect
	let f = File::open("the_code.rs").expect("error: the code hasn't been received yet!");
}
```
##### Propagating errors in Rust
- Sometimes, we want our functions to return a `Result` type and give their callers discretion over handling any errors
- The following code snippet is a useful example of how to do so:
```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
	let f = File::open("pwd.txt");
	let mut f = match f {
		Ok(file) => file,
		Err(e) => return Err(e), // returns the error early
	};

	let mut s = String::new();
	match f.read_to_string(&mut s) {
		Ok(_) => Ok(s), // on success, returns the username as a String
		Err(e) => Err(e), // on failure, returns an error
	}
}
```
- Again, this seems exceedingly verbose - thankfully, Rust provides us with another mechanism to cut down on the verbosity of our match expressions
	- This is reminiscent of TypeScript; we can specify a `?` at the end of function calls that return a `Result` type (this last point is important - we can't use the `?` operator in functions that don't return a value of `Result` type)
		- If these calls return an error, then it's converted to the error type defined in the return type of the current function and then returned
		- Else, we proceed as normal
- Let's shorten our function from earlier with this new trick:
```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
	let mut s = String::new();
	File::open("pwd.txt")?.read_to_string(&mut s)?;
	Ok(s)
}
```
##### General guidelines for error handling
- Our compiler isn't *that* smart, and doesn't realize that something like `"127.0.0.1".parse()` wouldn't return an error in any case
	- Here, it's good practice to use `unwrap` because the `parse` function returns a `Result` item by default, and we have to handle this somehow
- In cases where a failure is an expected possibility, we should return `Result` types rather than explicitly panicking. These errors are expected and recoverable, and we want to convey this to our consumers
- Each of the functions that we expose has a *contract* that guarantees certain behavior if callers follow that contract
	- If the contract is violated (i.e. by an out-of-bounds access for indexing a vector in Rust), we should panic
	- But we don't need to be super verbose in our checks for input validity - Rust's type-checking system gets us most of the way there
		- We can be smart here - i.e. using the `u32` integer type in our function definition to ensure that parameters are never negative
- Now, let's say that we wanted our input to be an integer between 1 and 100. We don't want to check this explicitly every time a user calls our function, so we define a struct that we can use as our input type:
```rust
pub struct Guess {
	value: u32,
}

impl Guess {
	// setter
	pub fn new(value: u32) -> Guess {
		if value < 1 || value > 100 {
			panic!("guess value must be in range [1, 100], got: {}", value);
		}

		Guess {
			value
		}
	}

	// getter
	pub fn value(&self) {
		// this is private by default, exposing it here
		self.value
	}
}
```
> [!note]- References
> - Chapter 9 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.