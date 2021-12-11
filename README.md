# Rust language reference

## Installation

Go to the rust [website](https://rust-lang.org) and follow the installation steps

## Hello world example

1. Create a file called `hello.rs`
2. Add the lines:

```rs

main() {
  println!("Hello, world!);
}

```

3. From the terminal run `rustc hello.rs`

## Starting real projects

1. Create a new folder
2. Navigate into the folder using the terminal and run `cargo init`. This will create a default project & file structure.
3. The function `main` within the file `src/main.rs` is the program's entry point
4. To run the project type `cargo run` into the terminal
5. To Build the project type `cargo build` into the terminal
6. To build an optimized release for production, type `cargo build --release` into the terminal

## Importing & Exporting functions

Export a function for use in other files by using `pub` as a prefix:

```rs
pub fn run() {
  // some code...
}
```

Importing the functions from another file is done through the `mod` (module) command. Executing a function from another file is done using the `file_name::function_name();` command e.g. `print::run()`

```rs
// Import print file/module
mod print;

fn main() {

    // run an function from the imported file
    print::run();
}
```

Using the standard library

- In order to reduce verboseness you can include functions from standard library at the top of a file

```rs
use std::mem;
```

## Printing to the console

Rust's print function, `println!("Hello")`, can only print strings. To print other data types we need to use a template string:

```rs
  // Print to console
  println!("Hello from the print.rs file");

  // Print to console
  println!("Number: {}", 1);

  // Basic formatting
  println!("Item 1: {}, Item 2: {}", 8, "string");

  // Positional arguments
  println!(
    "{0} is from {1} and {0} likes to {2}",
    "Rikus", "NZ/SA", "surf"
  );

  // Named arguments
  println!(
    "{name} likes to {activity}",
    name = "Rikus",
    activity = "surf"
  );

  // placeholder traits
  println!("Binary: {:b} Hex: {:x} Octal: {:o}", 10, 10, 10);

  // placeholder for debug trait
  println!("{:?}", (12, true, "hello"));

  // Basic math
  println!("10 + 10 = {}", 10 + 10);
```

## Variables

- Variables hold primitive data or references to data.
- Variables are immutable by DEFAULT
- Rust is a block-scoped language
- To make a variable mutable you declare it with the `mut` prefix
- The convention in rust for variables is `snake_case`, don't use

```rs

pub fn run() {
  let name = "Rikus";
  let mut age = 28;

  age = 29;
  println!("My name is {} and I am {}", name, age);

  // Assign multiple vars
  let (my_name, my_age) = ("Rikus", 28);
  println!("{} is {}", my_name, my_age);
}
```

### Constants

- We can assign constants variables using the `const` command for things that are fixed and used throughout a program. E.g API keys.
- Constants should be in `SCREAMING_SNAKE_CASE`

```rs
const API_KEY = "sd7er-8kjh-2nv2-xk912-fdy6-dhh8-abe7"
```

## Data types

Rust has the following primitive types:

- Integers: u8, i8, u16, i16, u32, i32, u64, i64, u128, i128
  - The number is the number of bits they take in memory
  - The u/i represents whether they are signed (positive or negative) or unsigned (just the number)
- Floats: f32, f64
- Boolean: bool
- Characters: char
- Tuples
- Arrays (have a fixed length)

Rust is a statically typed language, which means that it must know they types of all variables at compile time. However, the compiler can usually infer what type we want to use based on the value and how we use it.

```rs
pub fn run() {

  // default is i32
  let x = 1;

  // default is f64
  let y = 2.5;

  // add explicit type
  let z: i64 = 1000000000000;

  // find max size
  println!("Max i32: {}", std::i32::MAX);
  println!("Max i64: {}", std::i64::MAX);

  // boolean
  let is_active: bool = true;

  // get boolean from expression
  let is_greater: bool = 10 > 5;


  // char
  let a1 = 'a';
  let face = '\u{1F600}';

  println!("{:?}", (x, y, z, is_active, is_greater, a1, face));
}
```

## Strings

- Primitive `str` is an immutable fixed-length string somewhere in memory.
- `String` is a growable, heap allocated data structure - use when you need ot modify or own string data.

```rs
pub fn run() {

  // Primitive str
  let _primitive_hello = "Hello";

  // Growable string
  let mut greeting = String::from("Hello ");

  // push a SINGLE char onto the end of the string
  greeting.push('W');

  // push a string onto the end of the string
  greeting.push_str("orld!");

  // Capacity in bytes
  println!("Capacity: {}", greeting.capacity());

  // Get length
  println!("Length: {}", greeting.len());

  // Check if empty
  println!("Is empty: {}", greeting.is_empty());

  // Contains
  println!("Contains 'World': {}", greeting.contains("World"));

  // Replace
  println!("Replace: {}", greeting.replace("World", "There"));

  // Loop through string by whitespace
  for word in greeting.split_whitespace() {
    println!("{}", word);
  }

  // Create string with capacity
  let mut s = String::with_capacity(10);
  s.push('a');
  s.push('b');

  println!("{}", s);

  // Assertion testing
  assert_eq!(10, s.capacity());
}
```

## Tuples

- Tuples group together values of different types
- Max of 12 elements

```rs
pub fn run() {
  let person: (&str, &str, i8) = ("Rikus", "NZ", 28);
  println!("{} is from {} and is {}", person.0, person.1, person.2);
}
```

## Arrays

- In Rust, Arrays are fixed lists where elements are the same data types

```rs
use std::mem;

pub fn run() {
  let mut numbers: [i32; 5] = [1, 2, 3, 4, 5];

  // Re-assign value
  numbers[2] = 20;

  println!("{:?}", numbers);

  // Get single value
  println!("Single value: {}", numbers[0]);

  // Get array length
  println!("Array length: {}", numbers.len());

  // Arrays are stack allocated
  println!("Array occupies {} bytes", mem::size_of_val(&numbers));

  // Get slice
  let slice: &[i32] = &numbers;
  let smaller_slice: &[i32] = &numbers[0..2];

  println!("Slice: {:?}", slice);
  println!("Smaller slice: {:?}", smaller_slice);
}
```

## Vectors

- A vector is an array that can be shortened or lengthened.
- We will use them more than arrays.
- Their use is quite similar to arrays

```rs
use std::mem;

pub fn run() {
  let mut numbers: Vec<i32> = vec![1, 2, 3, 4, 5];

  // Re-assign value
  numbers[2] = 20;

  // Add on to vector
  numbers.push(5);
  numbers.push(6);

  // Pop off last value
  numbers.pop();

  println!("{:?}", numbers);

  // Get single value
  println!("Single value: {}", numbers[0]);

  // Get vector length
  println!("Vector length: {}", numbers.len());

  // vectors are stack allocated
  println!("Vector occupies {} bytes", mem::size_of_val(&numbers));

  // Get slice
  let slice: &[i32] = &numbers;
  let smaller_slice: &[i32] = &numbers[0..2];

  println!("Slice: {:?}", slice);
  println!("Smaller slice: {:?}", smaller_slice);

  // Loop through vector values
  for x in numbers.iter() {
    println!("Number: {}", x);
  }

  // Loop & mutate values
  for x in numbers.iter_mut() {
    *x = *x * 2;
  }

  println!("Numbers Vec: {:?}", numbers);
}
```

## Conditionals

- Check on the condition of something and act on it.

```rs

pub fn run() {
  let known_person_of_age: bool = false;
  let id_checked: bool = true;
  let age: u8 = 19;

  // if-else
  if known_person_of_age {
    println!("You are allowed to enter the club");
  } else if !id_checked{
    println!("We need to check your ID");
  } else if age >= 18 {
    println!("You are allowed to enter the club");
  } else {
    println!("You are not allowed to enter the club");
  }

  // short hand if-else
  let is_of_age = if age >= 18 { true } else { false };
  println!("Is of age: {}", is_of_age);
}

```

## Loops

- Loops are used to iterate until a condition is met.

```rs
pub fn run() {
  let mut count = 0;

  // Infinite loop
  loop {
    count += 1;
    println!("Number: {}", count);

    if count == 20 {
      break;
    }
  }

  // While loop (FizzBuzz)
  while count <= 100 {
    if count % 15 == 0 {
      println!("fizzbuzz");
    } else if count % 3 == 0 {
      println!("fizz");
    } else if count % 5 == 0 {
      println!("buzz");
    } else {
      println!("{}", count);
    }

    // Increment
    count += 1;
  }

  // for range
  for x in 0..100 {
    if x % 15 == 0 {
      println!("fizzbuzz");
    } else if x % 3 == 0 {
      println!("fizz");
    } else if x % 5 == 0 {
      println!("buzz");
    } else {
      println!("{}", x);
    }
  }
}
```

## Functions

- Used to store blocks of code for reusability

```rs
pub fn run() {
  greeting("Hello", "Rikus");

  // Bind function values to variables
  let get_sum = add(5, 5);
  println!("Sum: {}", get_sum);

  // Closure
  let n3 = 10;
  let add_nums = |n1: i32, n2: i32| n1 + n2 + n3;
  println!("C Sum: {}", add_nums(3, 3));
}

fn greeting(greet: &str, name: &str) {
  println!("{} {} nice to meet you.", greet, name);
}

fn add(n1: i32, n2: i32) -> i32 {
  return n1 + n2
}

```

## Reference pointers

- Used to point to a reference in memory
- With non-primitives, if you assign another variable to a piece of data, the first variable will no longer hold that value. You'll need to use a reference (&) to point to the resource.

```rs
pub fn run() {
  // Primitive array
  let arr1 = [1, 2, 3];
  let arr2 = arr1;

  println!("Values: {:?}", (arr2, arr1));

  // Vector
  let vec1 = vec![1, 2, 3];
  let vec2 = &vec1; 

  println!("Values: {:?}", (vec2, &vec1));
}
```
## Structs

- Structs are used to create custom data types. Similar to classes in Javascript

```rs
// Traditional struct
struct Color {
  red: u8,
  green: u8,
  blue: u8,
}

// Tuple struct
struct TupleColor(u8, u8, u8);

// Struct with a constructor   
struct Person {
  first_name: String,
  last_name: String,
}

impl Person {
  // Construct a Person
  fn new(first: &str, last: &str) -> Person {
    Person {
      first_name: first.to_string(),
      last_name: last.to_string(),
    }
  }

  fn full_name(&self) -> String {
    format!("{} {}", self.first_name, self.last_name)
  }

  fn set_last_name(&mut self, last: &str) {
    self.last_name = last.to_string();
  }

  fn to_tuple(self) -> (String, String) {
    (self.first_name, self.last_name)
  }
}

pub fn run() {
  let mut c = Color {
    red: 255,
    green: 0,
    blue: 0,
  };

  c.red = 200;

  println!("Color: {} {} {}", c.red, c.green, c.blue);

  let mut t = TupleColor(255, 0, 0);

  t.0 = 200;

  println!("Color: {} {} {}", t.0, t.1, t.2);

  let mut p = Person::new("John", "Doe");

  println!("Person: {} {}", p.first_name, p.last_name);

  println!("Person: {}", p.full_name());
  

  // Change the name
  p.set_last_name("Williams");

  println!("Person: {}", p.full_name());
  
  println!("Person Tuple: {:?}", p.to_tuple());
}

```

## Enums

Enums are types which have a few definite values

```rs
enum Movement {
  // variants
  Up,
  Down,
  Left,
  Right,
}

fn move_avatar(m: Movement) {
  // perform action depending on the variant
  match m {
    Movement::Up => println!("Avatar moving up"),
    Movement::Down => println!("Avatar moving down"),
    Movement::Left => println!("Avatar moving left"),
    Movement::Right => println!("Avatar moving right"),
  }
}

pub fn run() {

  let avatar1 = Movement::Up;
  let avatar2 = Movement::Down;
  let avatar3 = Movement::Left;
  let avatar4 = Movement::Right;

  move_avatar(avatar1);
  move_avatar(avatar2);
  move_avatar(avatar3);
  move_avatar(avatar4);
}
```

