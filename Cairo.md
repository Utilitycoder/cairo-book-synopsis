# Variables and Mutability in Cairo

## Key Points:
1. Variables in Cairo are immutable by default.
2. Cairo uses an immutable memory model.
3. The `mut` keyword can be used to make variables mutable.
4. Shadowing allows reusing variable names.

## Variables Basics:
- Use `let` keyword to create a variable:
  ```cairo
  let x = 5;
  ```
- Type inference is available, but you can specify types:
  ```cairo
  let y: u8 = 6;
  ```

## Mutability:
- By default, variables are immutable.
- To make a variable mutable, use the `mut` keyword:
  ```cairo
  let mut x = 5;
  x = 6; // This is allowed
  ```

## Shadowing:
- You can declare a new variable with the same name as a previous one:
  ```cairo
  let x = 5;
  let x = x + 1; // x is now 6
  ```
- Shadowing allows changing the variable's type:
  ```cairo
  let x: u64 = 2;
  let x: felt252 = x.into(); // x is now a felt252
  ```

## Constants:
- Declared using the `const` keyword
- Must have type annotation
- Can be declared in any scope, including global scope
- Cannot be the result of a value that can only be computed at runtime

Remember: Cairo's immutable memory model makes certain types of bugs impossible, but it requires a different approach to variable management compared to other languages.


## Data Types in Cairo

Cairo, as a statically-typed language, offers a variety of data types to ensure type safety and efficient memory usage. Here's a comprehensive overview of the main data types in Cairo:

### Scalar Types

1. **Felt (Field Element)**
   - The fundamental type in Cairo
   - Represents elements in a finite field
   - Range: 0 to P-1, where P is a large prime number (2^251 + 17 * 2^192 + 1)
   - Example: `let x: felt252 = 5;`

2. **Integers**
   - Fixed-size types: u8, u16, u32, u64, u128, u256
   - Unsigned integers with different bit widths
   - Example: `let age: u8 = 30;`

3. **Boolean**
   - Represents true or false
   - Takes up one felt in memory
   - Example: `let is_valid: bool = true;`

### Compound Types

1. **Tuples**
   - Group multiple values of different types
   - Fixed length, immutable
   - Example: `let coordinates: (u32, u32) = (10, 20);`

2. **Arrays**
   - Fixed-size collection of elements of the same type
   - Size must be known at compile time
   - Example: `let numbers: Array<u8> = array![1, 2, 3, 4, 5];`

3. **Structs**
   - Custom data types to group related data
   - Fields can have different types
   - Example:
     ```cairo
     struct Person {
         name: felt252,
         age: u8,
     }
     let person = Person { name: 'Alice', age: 30 };
     ```

### Special Types

1. **Option<T>**
   - Represents an optional value
   - Can be Some(T) or None
   - Useful for handling potential absence of values

2. **Result<T, E>**
   - Represents either success (Ok(T)) or failure (Err(E))
   - Used for error handling

3. **ContractAddress**
   - Represents an address on the Starknet network
   - Used for interacting with other contracts

### Type Aliases

- Create custom names for existing types
- Improves code readability
- Example: `type Price = u128;`

### Type Inference

- Cairo can often infer types based on usage
- Explicit type annotations can be omitted in many cases
- Example: `let x = 5; // Inferred as felt252`

### Type Conversion

- Use the `into()` method for safe conversions
- Example: `let y: u16 = x.into();`

Remember that Cairo's type system is designed to prevent common programming errors and ensure efficient execution on the Starknet platform. Understanding and properly using these types is crucial for writing robust and performant Cairo code.



## Basic Function Structure


- Functions are defined using the `fn` keyword, followed by the function name and parentheses.
- Function bodies are enclosed in curly brackets `{}`.
- Cairo uses snake_case for function and variable names.


Example:
```rust
fn another_function() {
    println!("Another function.");
}

fn main() {
    println!("Hello, world!");
    another_function();
}
```

## Function Order
- Functions can be defined before or after the `main` function.
- Cairo doesn't care about the order of function definitions.

## Statements and Expressions
- **Statements**: Instructions that perform actions but don't return a value.
- **Expressions**: Evaluate to a resultant value.

Example of a statement:
```rust
let y = 6;
```

Statements don't return values, so you can't assign a `let` statement to another variable:
```rust
let x = (let y = 6); // This will result in an error
```

## Functions with Return Values
- Return types are declared after an arrow `->`.
- The return value is usually the value of the final expression in the function body.
- You can use the `return` keyword for early returns.

Example:
```rust
fn five() -> u32 {
    5
}

fn main() {
    let x = five();
    println!("The value of x is: {}", x);
}
```

## Important Notes on Return Values
1. The last expression in a function is implicitly returned.
2. Adding a semicolon to the last expression turns it into a statement, which doesn't return a value.

Example of a function with parameters and return value:
```rust
fn plus_one(x: u32) -> u32 {
    x + 1
}
```

If you add a semicolon to the end of `x + 1;`, it becomes a statement and will result in a compilation error due to mismatched return types.

## Function Parameters
- Parameters are specified in the function signature.
- Each parameter must have a type declared.

Example:
```rust
fn print_labeled_measurement(value: i32, unit_label: felt252) {
    println!("The measurement is: {}{}", value, unit_label);
}
```

In this example, `value` is an `i32` and `unit_label` is a `felt252`.

Remember that Cairo is an expression-based language, so understanding the difference between statements and expressions is crucial for writing effective Cairo code.


## Function Overloading

Cairo does not support function overloading. This means you cannot define multiple functions with the same name but different parameter types. However, you can achieve similar functionality by using generics.

Example:
```rust
fn print_value<T>(value: T) {
    println!("The value is: {}", value);
}

fn main() {
    print_value(5);
    print_value("Hello");
}
```

In this example, the `print_value` function is defined with a generic type `T`. This allows it to accept any type of value, effectively achieving a form of polymorphism.


## Implicit Return

In Cairo, the `return` keyword is optional for the last expression in a function. If omitted, the last expression is implicitly returned.

Example:
```rust
fn add_one(x: u32) -> u32 {
    x + 1
}
```

In this example, the `add_one` function takes a `u32` and returns a `u32`. The last expression, `x + 1`, is implicitly returned, so the `return` keyword is not needed.

## Type Inference in Function Returns

Cairo can often infer the return type of a function if it's not explicitly specified. However, it's generally considered good practice to explicitly declare return types for clarity.


## Functions as Expressions

In Cairo, you can use functions as expressions. This means you can assign the result of a function call directly to a variable or use it in other expressions.

Example:
```rust
let result = five() + 1;
```


## Nested Functions

You can also define functions inside other functions, known as nested functions. These functions are only available within the scope of the parent function.

Example:
```rust
fn outer_function() {
    fn inner_function() {
        // Function body
    }
    // Call inner_function here
}
```

In this example, `inner_function` is defined within `outer_function`. It can only be called from within `outer_function`.

## Function Visibility

Cairo uses the `pub` keyword to control function visibility. By default, functions are private and cannot be accessed outside their module.

Example:
```rust
mod my_module {
    pub fn public_function() {
        // Function body
    }

    fn private_function() {
        // Function body
    }
}
```

In this example, `public_function` is public and can be accessed from outside the module, while `private_function` is private and cannot be accessed outside the module.


## Function Attributes

Cairo supports function attributes, which are metadata attached to functions. These are written with a `#` symbol before the function definition.

Example:
```rust
#[inline(always)]
fn frequently_called_function() {
    // Function body
}
```

In this example, the `#[inline(always)]` attribute tells the compiler to always inline this function.

This `inline` attribute suggests to the compiler that this function should always be inlined where it's called.

Remember that writing clear, concise functions is key to maintaining readable and efficient Cairo code. It's often beneficial to break down complex operations into smaller, well-named functions to improve code clarity and reusability.

--------------------------------

# Comments in Cairo

## Single-line Comments

Single-line comments in Cairo start with `//` and continue until the end of the line:

```rust
// This is a single-line comment
```

## Multi-line Comments

Cairo doesn't have a specific multi-line comment syntax. For longer comments, use multiple single-line comments:

```rust
// So we're doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what's going on.
```

## Inline Comments

Comments can be placed at the end of lines containing code:

```rust
fn main() -> felt252 {
    1 + 4 // return the sum of 1 and 4
}
```

## Preferred Comment Style

It's generally preferred to place comments on a separate line above the code being explained:

```rust
fn main() -> felt252 {
    // this function performs a simple addition
    1 + 4
}
```

## Item-level Documentation Comments

Used for documenting functions, implementations, traits, etc. These start with `///`:

```rust
/// Returns the sum of `arg1` and `arg2`.
/// `arg1` cannot be zero.
///
/// # Panics
///
/// This function will panic if `arg1` is `0`.
///
/// # Examples
///
/// ```
/// let a: felt252 = 2;
/// let b: felt252 = 3;
/// let c: felt252 = add(a, b);
/// assert(c == a + b, "Should equal a + b");
/// ```
fn add(arg1: felt252, arg2: felt252) -> felt252 {
    assert(arg1 != 0, 'Cannot be zero');
    arg1 + arg2
}
```

## Module Documentation Comments

These provide an overview of the entire module. They're placed above the module they're describing and start with `//!`:

```rust
//! # my_module and implementation
//!
//! This is an example description of my_module and some of its features.
//!
//! # Examples
//!
//! ```
//! mod my_other_module {
//!   use path::to::my_module;
//!
//!   fn foo() {
//!     my_module.bar();
//!   }
//! }
//! ```
mod my_module { // rest of implementation...
}
```

These comment styles in Cairo help developers write clear, well-documented code that is easier to understand and maintain.


# Control Flow in Cairo

## if Expressions

Basic if expressions allow code to branch based on conditions:

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

### Multiple Conditions with else if

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

### Using if in let Statements

```rust
let condition = true;
let number = if condition { 5 } else { 6 };
```

## Loops

Cairo provides several ways to handle repetitive tasks.

### loop Expression

The simplest form of loop that repeats forever until explicitly stopped:

```rust
fn main() {
    let mut counter = 0;

    loop {
        if counter == 10 {
            break;
        }
        counter += 1;
    }
}
```

### Returning Values from Loops

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };
}
```

### while Loops

Condition-based loops:

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);
        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

# Error Handling in Cairo

Cairo's error handling is designed to be explicit and help prevent bugs. The language provides several mechanisms for handling errors.

## Result Type

The `Result` enum is used for recoverable errors:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Using Result

```rust
fn divide(numerator: u64, denominator: u64) -> Result<u64, felt252> {
    if denominator == 0 {
        return Result::Err('Division by zero');
    }
    Result::Ok(numerator / denominator)
}

fn main() {
    match divide(10, 2) {
        Result::Ok(value) => println!("Result: {}", value),
        Result::Err(error) => println!("Error: {}", error),
    }
}
```

## The ? Operator

The `?` operator provides a convenient way to handle errors:

```rust
fn operation() -> Result<u64, felt252> {
    let result = divide(10, 2)?;  // Returns early if Err
    Ok(result + 1)
}
```

## Panic

For unrecoverable errors, Cairo provides the `panic` mechanism:

```rust
fn panic_example(value: u64) {
    if value == 0 {
        panic!("Value cannot be zero");
    }
}
```

# Testing in Cairo

Cairo has built-in support for testing through its test framework.

## Writing Tests

Tests are functions marked with the `#[test]` attribute:

```rust
#[test]
fn it_works() {
    let result = 2 + 2;
    assert(result == 4, 'addition problem');
}
```

## Test Organization

### Unit Tests

Unit tests are typically placed in the same file as the code they're testing:

```rust
fn add_two(a: u64) -> u64 {
    a + 2
}

#[test]
fn test_add_two() {
    let result = add_two(2);
    assert(result == 4, 'add_two failed');
}
```

### Integration Tests

Integration tests are placed in a separate `tests` directory:

```rust
// In tests/integration_test.cairo
use my_crate;

#[test]
fn test_external_api() {
    let result = my_crate::public_api();
    assert(result.is_ok(), 'API call failed');
}
```

## Running Tests

Tests can be run using the `scarb test` command:

```bash
scarb test
```

# Modules and Project Organization

## Project Structure

A typical Cairo project structure:

```
my_project/
├── Scarb.toml
├── src/
│   ├── lib.cairo
│   ├── main.cairo
│   └── module_a/
│       └── mod.cairo
└── tests/
    └── integration_tests.cairo
```

## Modules

Modules help organize code into logical units:

```rust
// In src/lib.cairo
mod module_a;

// In src/module_a/mod.cairo
fn public_function() {
    // Implementation
}
```

## Visibility and Privacy

Control access to module items using `pub`:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    front_of_house::hosting::add_to_waitlist();
}
```

## Using External Crates

Add dependencies in `Scarb.toml`:

```toml
[package]
name = "my_project"
version = "0.1.0"

[dependencies]
external_crate = { version = "1.0" }
```

Then use them in your code:

```rust
use external_crate::some_function;

fn main() {
    some_function();
}
```

# Memory Model and Ownership

Cairo's memory model is unique due to its immutable nature and ownership system.

## Ownership Rules

1. Each value has an owner
2. There can only be one owner at a time
3. When the owner goes out of scope, the value is dropped

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;  // s1 is moved to s2
    // println!("{}", s1);  // This would cause an error
}
```

## References and Borrowing

References allow you to refer to a value without taking ownership:

```rust
fn calculate_length(s: @String) -> usize {
    s.len()
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(@s1);
}
```

## Snapshots

Snapshots provide a way to create immutable views of data:

```rust
fn print_string(s: @String) {
    println!("{}", s);
}

fn main() {
    let s = String::from("hello");
    print_string(@s);  // Create a snapshot
    // s is still valid here
}
```

# Traits in Cairo

Traits define shared behavior across types. They're similar to interfaces in other languages.

## Defining a Trait

```rust
trait Summary {
    fn summarize(self: @Self) -> felt252;
}
```

## Implementing a Trait

```rust
struct NewsArticle {
    headline: felt252,
    content: felt252,
}

impl Summary for NewsArticle {
    fn summarize(self: @Self) -> felt252 {
        self.headline
    }
}
```

## Default Implementations

Traits can provide default behavior:

```rust
trait Summary {
    fn summarize(self: @Self) -> felt252 {
        'Default summary'
    }
}
```

## Trait Bounds

You can restrict generic types to those implementing specific traits:

```rust
fn notify<T: Summary>(item: @T) {
    println!("Breaking news! {}", item.summarize());
}
```

# Generic Types in Cairo

Generics allow you to write code that works with multiple types.

## Generic Functions

```rust
fn largest<T: PartialOrd>(list: @Array<T>) -> @T {
    let mut largest = @list[0];
    
    let mut i: usize = 1;
    while i < list.len() {
        if @list[i] > largest {
            largest = @list[i];
        }
        i += 1;
    }
    largest
}
```

## Generic Structs

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(self: @Self) -> @T {
        @self.x
    }
}
```

## Multiple Generic Parameters

```rust
struct Pair<T, U> {
    first: T,
    second: U,
}
```

# Collections in Cairo

## Arrays

Fixed-size collections of the same type:

```rust
fn array_example() {
    let numbers = array![1, 2, 3, 4, 5];
    let first = numbers[0];  // Accessing elements
    
    // Iterating over array
    let mut i = 0;
    while i < numbers.len() {
        println!("{}", numbers[i]);
        i += 1;
    }
}
```

## Span

A view into an array:

```rust
fn process_span(span: Span<felt252>) {
    let length = span.len();
    let first = *span.at(0);
}
```

# Advanced Pattern Matching

## Match Expression

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

## Pattern Matching with Option

```rust
fn plus_one(x: Option<u8>) -> Option<u8> {
    match x {
        Option::Some(n) => Option::Some(n + 1),
        Option::None => Option::None,
    }
}
```

## Match Guards

```cairo
fn match_number(n: felt252) {
    match n {
        x if x < 0 => println!("negative"),
        x if x > 0 => println!("positive"),
        _ => println!("zero"),
    }
}
```

# Contract Development

## Basic Contract Structure

```rust
#[starknet::contract]
mod MyContract {
    #[storage]
    struct Storage {
        balance: felt252,
        owner: ContractAddress,
    }

    #[constructor]
    fn constructor(ref self: ContractState, owner: ContractAddress) {
        self.owner.write(owner);
        self.balance.write(0);
    }

    #[external(v0)]
    fn get_balance(self: @ContractState) -> felt252 {
        self.balance.read()
    }
}
```

## Events

```rust
#[starknet::contract]
mod MyContract {
    #[event]
    #[derive(Drop, starknet::Event)]
    enum Event {
        Transfer: Transfer,
    }

    #[derive(Drop, starknet::Event)]
    struct Transfer {
        from: ContractAddress,
        to: ContractAddress,
        value: u256,
    }
}
```

## Storage Access

```rust
#[starknet::contract]
mod MyContract {
    #[storage]
    struct Storage {
        map: LegacyMap::<ContractAddress, u256>,
    }

    #[external(v0)]
    fn set_value(ref self: ContractState, address: ContractAddress, value: u256) {
        self.map.write(address, value);
    }

    #[external(v0)]
    fn get_value(self: @ContractState, address: ContractAddress) -> u256 {
        self.map.read(address)
    }
}
```

# Best Practices and Patterns

## Error Handling Pattern

```rust
// Define custom errors
#[derive(Drop)]
enum CustomError {
    InvalidInput,
    Unauthorized,
}

// Use Result with custom error type
fn process_input(value: u64) -> Result<u64, CustomError> {
    if value == 0 {
        return Result::Err(CustomError::InvalidInput);
    }
    Result::Ok(value * 2)
}
```

## Constant Definitions

```rust
const MAX_POINTS: u64 = 100_000;
const CONTRACT_VERSION: felt252 = 'v1.0.0';
```

## Safe Math Operations

```rust
use traits::CheckedAdd;

fn safe_add(a: u64, b: u64) -> Result<u64, felt252> {
    match a.checked_add(b) {
        Option::Some(sum) => Result::Ok(sum),
        Option::None => Result::Err('overflow'),
    }
}
```

# Advanced Cairo Concepts

## Decorators and Attributes

Cairo uses decorators (attributes) to modify behavior of functions and types:

```rust
// Contract interface decorator
#[starknet::interface]
trait IERC20 {
    fn total_supply(self: @ContractState) -> u256;
}

// Implementation decorator
#[starknet::contract]
mod ERC20 {
    // Storage decorator
    #[storage]
    struct Storage {
        total_supply: u256,
    }
}

// Test decorator
#[test]
#[available_gas(2000000)]
fn test_function() {
    // Test implementation
}
```

## Advanced Storage Patterns

### Storage Maps

```rust
#[starknet::contract]
mod AdvancedStorage {
    #[storage]
    struct Storage {
        // Simple mapping
        balances: LegacyMap::<ContractAddress, u256>,
        
        // Nested mapping
        allowances: LegacyMap::<(ContractAddress, ContractAddress), u256>,
        
        // Complex mapping
        user_data: LegacyMap::<ContractAddress, (u256, bool, felt252)>,
    }
}
```

### Storage Access Patterns

```rust
#[starknet::contract]
mod SafeStorage {
    use starknet::ContractAddress;

    #[storage]
    struct Storage {
        owner: ContractAddress,
        data: LegacyMap::<u256, u256>,
    }

    #[generate_trait]
    impl StorageAccess of StorageAccessTrait {
        fn ensure_owner(self: @ContractState) {
            let caller = starknet::get_caller_address();
            assert(caller == self.owner.read(), 'Not owner');
        }

        fn get_data(self: @ContractState, key: u256) -> u256 {
            self.data.read(key)
        }

        fn set_data(ref self: ContractState, key: u256, value: u256) {
            self.ensure_owner();
            self.data.write(key, value);
        }
    }
}
```

## Advanced Contract Patterns

### Upgradeable Contracts

```rust
#[starknet::contract]
mod UpgradeableContract {
    use starknet::ClassHash;

    #[storage]
    struct Storage {
        implementation: ClassHash,
    }

    #[external(v0)]
    fn upgrade(ref self: ContractState, new_implementation: ClassHash) {
        // Verify caller is authorized
        self.assert_only_owner();
        // Update implementation
        self.implementation.write(new_implementation);
    }
}
```

### Proxy Pattern

```rust
#[starknet::contract]
mod Proxy {
    use starknet::{ClassHash, ContractAddress};

    #[storage]
    struct Storage {
        implementation: ClassHash,
    }

    #[external(v0)]
    fn __default__(ref self: ContractState) {
        let implementation = self.implementation.read();
        // Delegate call to implementation
        starknet::library_call(
            implementation,
            selector!("handle"),
            calldata_span
        );
    }
}
```

## Advanced Types and Traits

### Associated Types

```rust
trait Container {
    type Item;
    fn add(ref self: ContractState, item: Self::Item);
    fn get(self: @ContractState) -> Option<Self::Item>;
}
```

### Generic Traits

```rust
trait Convertible<T> {
    fn convert(self: @Self) -> T;
}

impl U8IntoU16 of Convertible<u16> for u8 {
    fn convert(self: @u8) -> u16 {
        (*self).into()
    }
}
```

## Advanced Error Handling

### Custom Error Types

```rust
#[derive(Drop)]
enum TokenError {
    InsufficientBalance: (u256, u256), // (required, available)
    Unauthorized: ContractAddress,
    InvalidAmount: u256,
}

type Result<T> = core::result::Result<T, TokenError>;

fn transfer(amount: u256) -> Result<()> {
    if amount == 0 {
        return Result::Err(TokenError::InvalidAmount(amount));
    }
    // ... rest of implementation
    Result::Ok(())
}
```

### Error Propagation Patterns

```rust
fn complex_operation() -> Result<u256> {
    // Using ? operator for error propagation
    let value1 = operation1()?;
    let value2 = operation2()?;
    
    // Custom error mapping
    operation3().map_err(|e| TokenError::Unauthorized(e))?;
    
    Result::Ok(value1 + value2)
}
```

## Testing Patterns

### Test Utilities

```rust
mod test_utils {
    use core::test::test_address;
    
    fn setup() -> (ContractState, ContractAddress) {
        let owner = test_address();
        let contract = deploy_contract();
        (contract, owner)
    }
    
    fn create_test_accounts() -> Array<ContractAddress> {
        // Create test accounts
        array![
            test_address(),
            test_address(),
            test_address()
        ]
    }
}
```

### Property-Based Testing

```rust
#[test]
#[available_gas(2000000)]
fn test_transfer_properties() {
    let (contract, sender) = test_utils::setup();
    let receivers = test_utils::create_test_accounts();
    
    // Test various transfer amounts
    let amounts = array![1, 100, 1000, 10000];
    
    let mut i = 0;
    while i < amounts.len() {
        let amount = amounts[i];
        assert_transfer_preserves_total_supply(contract, sender, receivers[0], amount);
        i += 1;
    }
}
```

## Performance Optimization

### Gas Optimization Patterns

```rust
#[starknet::contract]
mod OptimizedContract {
    #[storage]
    struct Storage {
        // Pack related data together
        packed_data: u256, // Contains multiple fields in bit ranges
    }
    
    fn pack_data(value1: u8, value2: u8, value3: u8) -> u256 {
        // Bit packing for efficient storage
        (value1.into() << 16) | (value2.into() << 8) | value3.into()
    }
    
    fn unpack_data(packed: u256) -> (u8, u8, u8) {
        // Unpack data efficiently
        let value1 = ((packed >> 16) & 0xFF).try_into().unwrap();
        let value2 = ((packed >> 8) & 0xFF).try_into().unwrap();
        let value3 = (packed & 0xFF).try_into().unwrap();
        (value1, value2, value3)
    }
}
```
