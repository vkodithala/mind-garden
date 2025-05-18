---
publish: "true"
date: 2025-05-18
---

##### How to write a test in Rust
- The bodies of test functions in Rust (and in any language, really) perform three functions:
	- (1) Set up the initial state
	- (2) Run the code that we want to test with this initial state
	- (3) Assert that the results are as expected
- As a review, attributes are metadata that convey information about Rust function
	- For example, we used the `derive` attribute to allow for structs to be pretty-printed in [[005 Structuring related data with structs]]
	- To change a function into a test function, we need to add `#[test]` on the line before `fn`
		- We can then run all tests with the `cargo test` command in our terminal. Then Rust builds a test runner binary that considers all functions with the test attribute (which is why we need to specify it in the first place)
- Each test is run in a new thread; when the main thread sees that a test thread has died, it'll assume that the test failed
	- When an `assert_eq` function panics, the test thread dies, but we can also manually prompt this by issuing a panic when intended functionality isn't met in one of our tests
- Here's an example where we intentionally create a test that fails:
```rust
#[cfg(test)]
mod tests {
	#[test]
	fn auto_fail() {
		panic!("abort!");
	}
}
```
##### Testing equality with Rust macros
- The `assert!` macro is provided by the standard library, and takes in a function that returns a boolean result
	- If the function's result is true, the macro does nothing and our test passes
	- Else, the macro calls a `panic!` which triggers a test failure
- A common way to use the `assert!` macro in Rust is to call it with an argument that uses `==` or `!=` to compare two values
	- So Rust's team created two helper functions to handle this (common) case: `assert_eq` and `assert_ne`
	- Both take in two values, (which we'll call `left` and `right` based on their respective positioning) and then check for their equality/inequality
	- A helpful feature of these macros is that they actually print the arguments on a failure so that we can debug our code more seamlessly
- Under the surface, these helpers use the `==` and `!=` operators, and print their values to the log on failure
	- Thus, any inputs that we pass in must implement the traits `PartialEq + Debug`, else we'll get a compile error
- Here's an example of a test that uses `partial_eq`:
```rust
fn check_identity_test(password: &str) -> bool {
	password == "muad-dib"
}

#[cfg(test)]
mod dune_tests {
	use super::*; // bringing parent module fns into scope
	
	#[test]
	fn check_identity_test_assert() {
		let true_pass = "muad-dib";
		// using assert
		assert!(check_identity_test(&true_pass));
	}
	#[test]
	fn check_identity_test_asserteq() {
		let true_pass = "muad-dib";
		// using assert_eq
		assert_eq!(check_identity_test(&true_pass), true)
	}
}
```
- Let's say that our `check_identity_test_assert` fails. Because we aren't using `assert_eq` here, Rust gives us a pretty un-informative error message
	- We may not like this, so Rust gives us a way to supplement this with our own error message
	- Any values passed outside of the 1-2 expected by `assert!`/`assert_eq!` are wrapped in a `format!` macro and printed as an error message
- Here's an example of how we'd make debugging our test from earlier easier:
```rust
mod dune_tests {
	use super::*;

	#[test]
	fn check_identity_test_assert() {
		let true_pass = "muad-dib";
		assert!(
			check_identity_test(&true_pass),
			"password was not correct. you passed in {}", true_pass
		);
	}
}
```
- But there are cases where we want to flip the functionality of tests on their head - i.e., we want to check whether a test panics rather than running successfully
	- The practical use cases of this are when we want to ensure that invalid inputs result in `panic!` being invoked
- To do this, we add the `should_panic` macro below our `test` macro and before our function definition
	- This macro can contain an optional `expected` value, which specifies a substring that must appear in the `panic!` message body in order for the test to return successfully
- Here's an example:
```rust
fn greater_than_100(value: u32) -> bool {
	if value <= 100 {
		panic!("received {}, not > 100!", value);
	}
	true
}

#[cfg(test)]
mod tests {
	use super::*;

	#[test]
	#[should_panic(expected = "not > 100!")]
	fn test_greater_than1_100() {
		greater_than_100(90);
	}
}
```
##### Controlling how tests are run
- By default, tests in Rust run on parallel threads, which may not be what you want if your tests use and update some kind of shared data (like data stored on disk - r/w to this would cause a data race!)
- We can actually configure our tests so that they all share one thread rather than spawning a thread/test, and can then use and update share data without consequence
	- The command for this is `cargo test -- --test-threads=1`
	- As a rule of thumb, any configurations that follow `--` go to the test binary, and those that precede it go to `cargo test`
- By default, Rust hides anything printed to the standard output (using `println!`) for tests that pass, and prints these out to the user for failing tests
	- If we want to see printed values from both (we usually only do in cases where we print stuff for debugging), we can do so by ading the `nocapture` flag
		- Like: `cargo test -- --nocapture`
- Sometimes, we want to only run a subset of all of our tests. We can do so by adding flags
	- If we run `cargo run {test_name}`, only tests with `{test_name}` as a substring will be run
		- It's important to understand that `{test_name}` is a substring. So if we indicate `cargo run add`, then two tests with the name `add_two` and `add_hundred` would be run, but something like `test-two` would not be
- We can also choose to explicitly ignore certain tests that might be expensive, by adding the `ignore` macro below our `test` macro like so:
```rust
#[test]
#[ignore]
fn sample_test() ...
```
- Now to run all 'ignored' tests, we'd run the command `cargo test -- --ignored`
##### Rules of thumb for organizing your test suite
- Unit tests are tests of specific interfaces/ functions - they allow us to quickly pinpoint where code is/isn't working as expected
	- They live within the `/src` directory
	- Convention tells us to create a module `tests` in each file to contain the test functions, and annotate it with `#[cfg(test)]` so that it isn't compiled when we run anything other than `cargo test` (and saves us time on builds)
- A quick note: Rust *does* allow you to test private functions, since technically your unit tests are in the same file as your application code and thus don't need to specify the `pub` modifier to import/run those functions
- The second type of test that we see in Rust is called an integration test. The purpose of these tests is to test how parts of your library work *together* rather than independently (the latter of which we leave to unit tests)
	- To create these tests, we create a `/tests` directory underneath our top-level project directory, and then make as many test files as we want to here
		- Every test file in the `tests` directory is compiled into its own crate
	- If we want to use any of the functions in our application code, we'll need to set them as `pub` (we can't use private functions here) and import them as crates with `extern crate {crate_name};` at the top of the test file
- Running integration tests can be done in one of two ways:
	- Along with the test of our tests with `cargo test`
	- In a specific integration test file with `cargo test --test {file_name}`
- We discussed earlier how Rust automatically compiles test files into crates. But what if we don't want that?
	- Let's say we had a file `tests/common.rs` that contained some common setup code that our test 'crates' use
	- To avoid this being compiled into a crate (and 'tested' along with our true tests), we can act like it has submodules by moving its code to `tests/common/mod.rs`
	- Then we can safely call its functions from any other file in our `tests` directory without that file itself being tested
- Note: projects that are simply binary crates can't have integration tests as they're meant to be run on their own
	- So Rust devs usually place the main functionality of their application in the library crate (which integration tests *can* test), and use `src/main.rs` (our binary crate) as more of a 'glue' with minimal application logic
> [!note]- References
> - Chapter 11 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.