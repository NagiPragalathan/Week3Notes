### **Ownership and Borrowing in Rust**

#### **Ownership in Rust:**

- **Ownership** is the most unique feature of Rust, and it enforces memory safety.
    - Every value in Rust has a variable that’s its owner.
    - Only one owner at a time.
    - When the owner goes out of scope, the value is dropped, automatically freeing the memory.

**Example:**

```rust
fn main() {
    let s1 = String::from("hello"); // s1 owns the string
    let s2 = s1; // Ownership of the string moves to s2; s1 is now invalid

    // println!("{}", s1); // This would cause an error because s1 no longer owns the value
    println!("{}", s2); // Valid because s2 now owns the value
}
```

#### **Borrowing in Rust:**

- **Borrowing** allows references to values without taking ownership.
    - **Immutable Borrowing:** Multiple immutable references are allowed.
    - **Mutable Borrowing:** Only one mutable reference is allowed, and no other references (immutable or mutable) are permitted while the mutable reference exists.

**Borrowing Rules:**

- You can have any number of immutable references.
- You can have only one mutable reference.
- No references (mutable or immutable) can exist when a mutable reference exists.
- No dangling references are allowed.

**Example:**

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // immutable borrow
    let r2 = &s; // another immutable borrow

    // let r3 = &mut s; // This would cause an error because a mutable borrow is not allowed with active immutable references

    println!("{}, {}", r1, r2); // This works fine

    let r3 = &mut s; // mutable borrow is allowed here because no immutable references are active
    println!("{}", r3);
}
```

#### **Drop and Copy Traits:**

- The `Drop` trait is called automatically when an object goes out of scope, deallocating the memory.
- The `Copy` trait is for types that can be duplicated trivially, such as integers.

**Example of Drop Trait:**

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // immutable borrow
    let r2 = &s; // another immutable borrow

    // let r3 = &mut s; // This would cause an error because a mutable borrow is not allowed with active immutable references

    println!("{}, {}", r1, r2); // This works fine

    let r3 = &mut s; // mutable borrow is allowed here because no immutable references are active
    println!("{}", r3);
}
```
**Example of Copy Trait:**

```rust
fn main() {
    let x = 5; // x is a simple integer and implements the Copy trait
    let y = x; // x is copied to y

    println!("x: {}, y: {}", x, y); // Both x and y are still valid
}
```
### **Data Structures and Iterators**

Rust provides several iterator methods for working with collections effectively. Iterators allow you to traverse and process data.

#### **Basic Iterators:**

- **`iter()`**: Creates an iterator that yields immutable references.
- **`iter_mut()`**: Creates an iterator that yields mutable references.
- **`into_iter()`**: Consumes the collection and yields ownership of each value.

**Example:**

```rust
fn main() {
    let v = vec![1, 2, 3];
    
    for val in v.iter() {
        println!("{}", val); // Iterates over immutable references
    }
}
```

#### **Iterator Adapters:**

Iterator adapters are methods that transform iterators into new iterators.

- **`map()`**: Applies a function to each element, returning an iterator of transformed values.
    
    ```rust
    fn main() {
      let numbers = vec![1, 2, 3];
      let squared: Vec<_> = numbers.iter().map(|x| x * x).collect();
      println!("{:?}", squared); // Outputs: [1, 4, 9]
    }
    ```
    
- **`filter()`**: Filters elements based on a condition, returning an iterator of elements that satisfy the condition.
    
    ```rust
    fn main() {
          let numbers = vec![1, 2, 3, 4, 5];
          let even_numbers: Vec<_> = numbers.into_iter().filter(|&x| x % 2 == 0).collect();
          println!("{:?}", even_numbers); // Outputs: [2, 4]
      }
    ```

    
- **`enumerate()`**: Adds an index to each element.
    
    ```rust
    fn main() {
        let names = vec!["Alice", "Bob", "Charlie"];
        for (index, name) in names.iter().enumerate() {
            println!("{}: {}", index, name); // Outputs: 0: Alice, 1: Bob, 2: Charlie
        }
    }
    ```
    
- **`peekable()`**: Allows you to peek at the next element without consuming it.
    
    ```rust
    fn main() {
        let mut iter = vec![1, 2, 3].into_iter().peekable();
        println!("{:?}", iter.peek()); // Outputs: Some(1)
    }
    ```

    
- **`take()`**: Takes only a specified number of elements.
    
    rust
   ```
   fn main() {
        let numbers = vec![1, 2, 3, 4, 5];
        let first_two: Vec<_> = numbers.into_iter().take(2).collect();
        println!("{:?}", first_two); // Outputs: [1, 2]
    }
   ```
    
- **`skip()`**: Skips a specified number of elements.
    
    ```rust
    fn main() {
        let numbers = vec![1, 2, 3, 4, 5];
        let remaining: Vec<_> = numbers.into_iter().skip(3).collect();
        println!("{:?}", remaining); // Outputs: [4, 5]
    }
    ```
   
- **`chain()`**: Combines two iterators.
    
    ```rust
    fn main() {
        let first = vec![1, 2, 3];
        let second = vec![4, 5, 6];
        let combined: Vec<_> = first.into_iter().chain(second.into_iter()).collect();
        println!("{:?}", combined); // Outputs: [1, 2, 3, 4, 5, 6]
    }
    ```

#### **Iterator Consumers:**

These methods consume the iterator and return results.

- **`collect()`**: Gathers all iterator results into a collection.
    
    ```rust
    fn main() {
        let numbers = vec![1, 2, 3];
        let doubled: Vec<_> = numbers.into_iter().map(|x| x * 2).collect();
        println!("{:?}", doubled); // Outputs: [2, 4, 6]
    }
    ```
    
- **`fold()`**: Accumulates values based on an operation.
    
    ```rust
    fn main() {
        let numbers = vec![1, 2, 3, 4];
        let sum = numbers.iter().fold(0, |acc, &x| acc + x);
        println!("Sum: {}", sum); // Outputs: Sum: 10
    }
    ```
    
- **`find()`**: Returns the first element that matches a condition.
    
    ```rust
    fn main() {
        let numbers = vec![1, 2, 3, 4, 5];
        if let Some(number) = numbers.into_iter().find(|&x| x == 3) {
            println!("Found: {}", number); // Outputs: Found: 3
        }
    }
    ```
    
- **`all()` and `any()`**: Check if all or any elements satisfy a condition.
    
    ```rust
    fn main() {
          let numbers = vec![1, 2, 3, 4, 5];
          let all_even = numbers.iter().all(|&x| x % 2 == 0);
          let any_even = numbers.iter().any(|&x| x % 2 == 0);
          println!("All even: {}, Any even: {}", all_even, any_even); // Outputs: All even: false, Any even: true
      }
    ```

    

### **Lifetimes in Rust**

Lifetimes prevent dangling references by ensuring that references are valid for as long as they are needed.

#### **Lexical Lifetimes (NLL):**

A feature introduced in Rust that makes the borrow checker more flexible, allowing some previously disallowed code to compile.

#### **Example of Lifetimes:**

```rust
fn main() {
    let x = 5;
    let r = &x;

    println!("r: {}", r); // `r` is valid because `x` is in scope and won't be dropped
}
```

### **Generics and Traits**

#### **Generics:**

Generics are used for writing reusable code. They allow types and functions to be written generically for multiple data types.

**Example:**

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}
```

#### **Traits:**

Traits define shared behavior between types. They are similar to interfaces in other languages.

**Example:**

```rust
trait Summary {
    fn summarize(&self) -> String;
}

struct NewsArticle {
    headline: String,
    content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}: {}", self.headline, self.content)
    }
}
```
#### **Trait Bounds:**

Trait bounds specify that a generic type must implement a particular trait.

**Example:**

```rust
fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}
```
