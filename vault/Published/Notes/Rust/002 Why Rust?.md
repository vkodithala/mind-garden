---
publish: "true"
date: 2025-05-04
---
- Why Rust? It's important to answer this question to motivate our learning
- Systems/low-level programming offers benefits like faster runtimes, but it's incredibly hard to work with because of subtle bugs that come from things like memory-safety issues
- Rust avoids these bugs with a 'smart' compiler that refuses to compile code with elusive bugs
	- This allows devs to focus on program logic rather than chasing down bugs
- Rust also brings contemporary developer tools to the systems programming world:
	- **Cargo:** a dependency manager that makes adding, compiling, and managing dependencies seamless
	- **Rustfmt:** a linter that ensures consistent coding style
	- **Rust language server:** a language server that powers code completion/inline error messages on IDEs
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
> [!note]- References
> - Chapter 1 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.