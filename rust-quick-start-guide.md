## Summary
### Ch1. Gettying Ready
- 1.1. Installing Rust
  - six-week rapid release
    - beta: where features live before going to stable, remaining in beta for years
    - nightly: 24h behind the dev code
  - cargo: the frontend to the whole rust compiler system
- 1.2. Starting a new project
  - create the skeleton
    - of a new program: cargo new foo
    - of a new library: cargo new --lib bar
  - 1.2.1. Project metadata
    - Cargo.toml
      - name, version, author, **a section for dependencies**
      - toml: a more well-defined and feature-complete version of the old .ini format
    - 1.2.1.1. Dependencies on libraries from **crates.io**(published)
      - ``` [dependencies] serde = "1.0.70" ```
    - 1.2.1.2. Dependencies on Git repositories
      - ``` [dependencies] thing = { git = "somewhere git-reconizable" } ```
    - 1.2.1.3. Dependencies on local libraries
      - ``` [dependencies] example = { path = "/path/to/example" } ```
  - 1.2.2. Automatically generated source files
    - main.rs: the boilerplate main.rs with main function
    - lib.rs: the lib boilerplate including a framework for automated tests
- 1.3. Compiling our project
  - cargo build in / under the directory containing Cargo.toml
    - everything it needs to know is in the metadata file
    - warnings
      - do not prevent the compile from succeeding
      - **hints for improving efficiency**
    - Cargo.lock
      - largely safe to ignore this file
      - a program: should be added to the version control system
      - a library: shouldn't be
    - target dir
      - containing all of the build artifacts and intermediate files
      - cargo build + run target/debug/foo == cargo run
  - 1.3.1. Debug and release builds
    - By default, cargo builds prog in debug mode
      - instrumented to work with the rust-gdb
      - skipping the compiler's optimization phase
    - cargo build --release for optimized version => target/release/foo
  - 1.3.2. Dynamic libraries, software distribution, and Rust
    - Rust avoids using dynamic libraries, excepting os libraries
      - =~ go build --static
- 1.4. Using crates.io
- 1.5. Summary
### Ch2. Basics of the Rust Language
- 2.1. Functions
  - 2.1.1. Defining a function
    - naming convention
      - do not start with a number
      - a name staring with an underscore should followed by at least one further character
    - '->' symbol followed by a return type ??
- 2.2. Modules
  - shorter version is similar with the convention of go package
  - 2.2.1. Defining a module
    - 2.2.1.1. A module as a section of a file
      - pub keyword: defining a public interface of the current module
    - 2.2.1.2. A module as a separate file
      - give our modules their own files to keep the code manageable
      - looking for one of files(module_b.rs | module_b/mod.rs)
        - using the whole file as the contents of the module_b
  - 2.2.2. Accessing module contents from outside
    - 2.2.2.1. Using the item's full name directly
      - std::path::Path
    - 2.2.2.2. Using the item's short name 
      - ```{ use std::path::Path; Path(); } ```
      - ``` use std::path; path::Path(); // similar with the Go package convention```
    - 2.2.2.3. Public and private module items
      - pub: makes the item attached to public, **private if omitted**
        - private is **not a security mechanism**
        - pub/priv distinctions to help us make it plain which parts are **intended for use outside**
- 2.3. Expressions
  - the idea of expressions: **combine values to produce new values**
  - 2.3.1. Literal expressions
  - 2.3.2. Operator expressions
  - 2.3.3. Array and tuple expressions
    - ```[0; 1000]```, which produces an array containing a thousand zero values
    - a_tuple.1, which produces the second value in a_tuple
      - intended to serve as lightweight ds containing multi values with diff types
      - ``` (5,)``` **need to include a comma after the value to make a tuple containing only one value**
  - 2.3.4. Block expressions / 2.3.5. Branch expressions
    - Valid case: ```if { 2+2 } == 4 && { 2+2; true } { println!("haha") } ```
    - Error case: ```if { 2+2 } == 4 && { 2+2; true; } { println!("ㅠㅠ") } ```
    - instruction with semicolon: **statement**
      - it will not produce a value
      - ** explicitly discarding the results of expressions allows the compiler to make inferences / optimizations**
      - **ends up () as it s resulting value** 함수형의 냄새가...
    - no semicolon only for the last instruction
  - 2.3.6. Loop expressions
    - 2.3.6.1 while loops
      - important for the block expression to change something sffecting the condition expression's result -> mutability와 연관
    - 2.3.6.2. for loops
      - runs its block expression once for each value produced by an iterator
        - 3..7: range expression, represents the values 3~6, except 7
      - ("abc", 2) tuple is not a iterator
- 2.4. Variables, types, and mutability
  - Compile error with unbounded vairables
  - By default, vairables are immutable, but mutable variables
  - Shadowing: multiple let statements with the same name
    - blocks new references to the old variable
  - 2.4.1. Type inference
- 2.5. Data structures
  - As like Golang, the final comma is recommended
  - unpulished members can be accessed 
    - only within the defined module directly
    - or by the public getter function
  - 2.5.1. Mutability of data structures
- 2.6. More about functions
  - 2.6.1. Parameters
  - 2.6.2. Return types
    - A function call is an expression, producing a result via 'return' or final expression
  - 2.6.3. Error handling
    - Result<success result, error> type
    - 2.6.3.1. Using Result to signal succes or failure
      - OK(()) -> Result<(), &'static str>
    - 2.6.3.2. Calling functions that return Result
      - ?: extracts the stored value from a successful Result, or returns the 'Result'
        - the return type of callee should be same as **Result<sth, same error type>**
      - expect: same as '?' for success, but terminate the program printing out an err msg
      - etc: calling an error handling function
- 2.7. Implementing behavior for types
  - implementation block(not a block expression): impl XXX {}
    - Function implementation on type we didn't create -> only if we create a trait
    - able to impl funcs on created data types within the same **project**, without using traits
### Ch3. The Big Ideas: Ownershipt and Borrowing
- 3.1. Scopre and ownership
  - 3.1.1. The stack
- 3.2. Transferring
- 3.3. Copying
- 3.4. Referencing
  - 3.4.1. Referencing immutably
  - 3.4.2. Referencing mutably
- 3.5. Accessing referenced data
- 3.6. The lifetime of referenced data

## Sample codes
### 2.2.1.1
```rust
pub mod module_a {
    pub fn a_thing() {
        println!("a thing");
    }
    pub fn a_second_thing() {
        a_thing();
        println!("another thing");
    }
}
```
