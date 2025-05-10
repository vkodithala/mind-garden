---
publish: "true"
date: 2025-05-05
---
- Collections point to data that's stored on the heap, which means that the amount of data stored can grow during runtime
##### The `Vector` data type
- Vectors are dynamic arrays of data of the same type that's stored on the heap
- We can initialize them in two ways:
	- As an empty array, where we need to specify their type - `let v: Vec<i32> = Vec::new();`
	- With initial values (using the `vec!` macro), where Rust can infer the type - `let v = vec![1, 2, 3];`
- We update a vector via the `push` method, like `v.push(5);`
- When a vector goes out of scope, it's dropped (like any other item in Rust), and its contents are also dropped during this cleanup process
- There are two ways to reference elements in vectors, shown below:
```rust
let v = vec![1, 2, 3, 4, 5];

let does_not_exist = &v[100]; // resulting type: &i32
let does_not_exist = v.get(100); // resulting type: Option<&i32>
```
- The first method causes the program to panic, but the second simply sets `does_not_exist` to `None`. We can handle this case via a `match` flow, like we discussed in [[004 Understanding ownership]]
- When we *do* acquire a valid reference, Rust still enforces that we can't have a mutable reference with any other references (mutable or immutable). For an example, see this code that causes a compiler error:
```rust
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0]; // immutable reference to the first element
v.push(6); // needs a mutable reference in order to push an element to the vector
```
- But why does `v.push(6)` require a mutable reference? This is related to the way that vectors work. When we don't have enough memory currently allocated to push the new element, we need to allocate a new location of the heap (usually around 2x as big as the first) and copy the elements over
	- This results in the immutable reference `&v[0]` becoming invalid
	- Hence why we can't have an immutable and mutable reference together in the same scope!
- Similar to the `push` method, we also have a `pop` method - which we won't go over in detail but should be pretty self-explanatory
- If we don't need to update the elements in a vector, we can iterate through them this way:
```rust
for i in &v {
	println!("{}", i); // don't need to dereference because `Display` (which println implements) does that implicitly
}
```
- If we *do* need to edit the elements, we do that like so:
```rust
for i in &mut v {
	*i += 50;
}
```
- We can use the enum data type to make a vector whose elements don't all belong to one type, but that belong to some group of types that we know beforehand
- For instance, let's say we have a spreadsheet and we know that the entries will be 1/3 types. We can create an array with these elements as follows:
```rust
enum SpreadsheetCell {
	Int(i32),
	Float(f64),
	Text(String),
}

let row = vec![
	SpreadsheetCell::Int(3),
	SpreadsheetCell::Float(3.1),
	SpreadsheetCell::Text(String::from("blue!")),
]
```
##### More on the `String` datatype
- Similarly to `Vec<T>`, we use the format `new` function like `String::new()` if we want to create a new, empty string
- Let's say that we want to create a string initialized with the contents "initial contents". We can do this in a few ways, all shown below:
```rust
let data = "initial contents";

// below are all equivalent ways to make a string "initial contents"
let s = data.to_string();
let s = "initial contents".to_string();
let s = String::from("initial contents");
```
- If we're initializing a string with data, the method that we use is simply based on preference
- There are a couple of ways that we can add to the end of mutable strings: using the `push_str(&str2)` method, and the `push(&char)` method, as shown below:
```rust
let mut s = String::from("foo");

fn foobar(s: &mut String) {
	let s2 = String::from("bar");
	s.push_str(&s2); // after this, s = "foobar"
}

fn fool(s: &mut String) {
	s.push('l'); // after this, s = "fool"
}
```
- Notice that `push_str` takes in a string slice/reference as an argument. That's because we don't want to take ownership of the parameter `s2` such that it can be used later in our code
- Now let's move on to string concatenation
	- The easiest way to concatenate strings is by using the `+` operator, which uses the `add(self, s: &str) -> String` method under the hood
		- We "add" an item of type `String` with an item of type `&str` (but if we pass something in of type `&String` Rust conveniently just converts it for us)
	- This can get lengthy if we're adding a bunch of strings. So we introduce the `format!` operator, which works exactly the same way as `println!` but returns a string
		- One key thing about `format!` is that doesn't take ownership of any of its parameters (unlike `+`, which takes ownership of the first)
- Here's an example of how we'd use the 2 methods that we've used to concatenate strings:
```rust
let s1 = "Darth ";
let s2 = "Vader";

let concat_1 = s1 + &s2; // after this point, s1 is invalid
let concat_2 = format!("{}{}", s1, s2); // s1 and s2 are both valid after
```
- Because Rust encodes UTF-8 in strings, it doesn't support the indexing mechanism
	- Under the hood, the `String` datatype is simply a wrapper over `Vec<u8>`. Let's say we have a string that's entirely in Unicode. Remember that Unicode scalar vectors take up 2 bytes of storage, so by calling on the 0th index of the string, we're in fact pulling in the first byte - which is certainly not what we want
		- To avoid errors like this, Rust prevents all indexing with its compiler
- Instead, Rust asks you to be more specific when using indices to create string slices
	- Rather than direct indexing, we can provide a range to index with that starts and ends on clear boundaries (if start, end aren't clear boundaries, then Rust refuses to compile)
- Here's an example:
```rust
let hello_eng = "Hello, world!";
let s = &hello_eng[0..1];

let hello_rus = "Здравствуйте";
let s = &hello_rus[0..4];
```
- Recall that the Russian text is comprised of Unicode vectors, which require 2 bytes/character. Thus we take a slice of bytes 0, 1, 2, and 3 to extract the first 2 "letters"
- Remember that strings can be represented in three ways: scalar values, bytes, and grapheme clusters (grapheme clusters = what we typically think of as characters)
- There are ways of iterating through strings to extract bytes and scalar values, but not grapheme clusters - though crates do exist to expose this functionality. See here:
```rust
for c in "hello".chars() {
	println!("{}", c);
}

for b in "hello".bytes() {
	println!("{}", b);
}
```
##### Hash maps in Rust
- Hash maps, like in any other programming language, store values mapped to specific keys
	- They're quite useful in storing information - i.e. if we were making a video game in Rust and wanted to keep track of player scores
- Like strings and vectors before them, hash maps have methods for empty initialization and initialization with some default values, which are shown below. A key difference here is that this data type isn't used by all programs and so we need to 'import' it before usage
```rust
use std::collections::HashMap;

let mut scores = HashMap::new()
scores.insert(String::from("Rebels"), 10);
scores.insert(String::from("Empire"), -1);

// an equivalent way of initializing `scores`
let teams = vec![String::from("Rebels"), String::from("Empire")];
let scores = vec![10, -1]
let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect(); // <_, _> is used for type inference of the <K, V> types
```
- For data types that implement the `Copy` trait (these are deep copied during aliasing - see [[004 Understanding ownership]] for more details), their data is copied into the hash map
	- But for those that don't, the hash map becomes the owner and previous references are invalid (we can avoid this by explicitly using `.copy()` on our data before the `insert` operation)
		- We can also pass references as values, but need to ensure that those references are valid for as long as the hash map itself is
- We can retrieve values from a HashMap using the `get` mechanism, which takes in a reference to a key and returns a value of type `Option<T>` that we must handle using a `match`
	- Alternatively, we can use a for loop to iterate through all the values in a hash map, as shown below:
```rust
...
// batch iteration
for (team, score) in &scores {
	println!("team: {} has score {}", team, score);
}

// `get` mechanism
let rebel_score = scores.get("rebels"); // key automatically dereffed
if let Some(score) = rebel_score {
	println("the rebellion's score is {}", score);
}
```
- There are three discrete cases that we might encounter when trying to update a hash map: (a) replacing values, (b) inserting values if one doesn't exist, and (c) updating a value based on the old value. Mechanisms for all three are shown below:
```rust
// updating an existing value (works by default with .insert() method)
scores.insert(String::from("empire"), 50);
println!("order 66 has been executed!");

// only adding a value if one doesn't already exist
scores.entry(String::from("jedi")).or_insert(100);
println!("Yoda has landed on dagobah!");
// if 'jedi' exists as a key, this returns a ref to the existing value
// if not, sets scores['jedi'] to 100

for team in scores.keys() {
	let count = map.entry(team); // returns &mut V where V = value
	*count += 1;
}
```

> [!note]- References
> - Chapter 8 of *The Rust Programming Language* by Steve Nichols and Nicole Klabnick.