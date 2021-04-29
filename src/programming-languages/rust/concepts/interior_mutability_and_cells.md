# Rust 中的可变与不可变以及 Cell 的使用

### 基本概念

Rust的[所有权(ownership)](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html)机制规定：Rust中的每个值都有一个被称为其所有者（owner）的变量，并且有且只能有唯一的所有者。

Rust中的[引用(references)](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html)允许使用值但不获取其所有权，这种操作也被称为所有权借用（borrowing）。通过`＆T`和`＆mut T`将引用分为两种：

- 不可变引用（`&T`），也被称为共享引用，所有者可以读取引用指向的数据，但不能修改数据。
- 可变引用（`&mut T`）也被称为独占引用，不能有别名，在同一时刻，同一个值不可能存在别的引用。

```rust
fn main() {

    let mut s = String::from("hello");
    let r1 = &mut s;
    let r2 = &mut s; // ERROR: 可变引用，不能有别名
    println!("{}, {}", r1, r2);
    
    let x = 1;
    let y = &mut x; // ERROR: 当有一个不可变值时，不能可变的借用它
}
```

Rust内存安全性基于以下规则：给定对象T，则只能具有以下之一：

- 对象有几个不可变的引用（`&T`），也称为别名（aliasing）。
- 对象有一个可变引用（`&mut T`），也称为可变性（mutability）。

这由Rust编译器强制执行。但是，在某些情况下，此规则不够灵活。有时需要对一个对象有多个引用并对其进行改变。

```rust
fn main() {
			
    let mut data = 1_i32;
    let p : &i32 = &data;	// 两个变量data和p访问同一块内存区域，称为别名
    data = 10;				// ERROR: 存在别名时，不能同时提供可变性
    println!("{}", *p);
}
```

在Rust中，一个变量是否是可变的，取决于是否用`mut`修饰变量绑定。如果我们用`let var : T`声明，那么`var`是不可变的；而且，var内部所有的成员也都是不可变的；如果我们用`let mut var : T`声明，那么`var`是可变的，相应的它的内部所有成员也都是可变的。

> 术语：继承/承袭可变性（Inherited Mutability），必须具有对变量的唯一访问权。

这样的话，如果有个结构体引用`&SomeStruct`，则`SomeStruct`的所有字段都是不可变的。

```rust
struct Foo {
    x: u32
}

fn print_foo(foo: &Foo) {
    println!("x={}", foo.x);
}

fn change_foo(foo: &Foo) {
    foo.x = foo.x * 2; // ERROR: 不允许改变数据
}
```

但在实际开发中，确实存在需要结构体中的某个字段可变的情况。

针对这些情况，Rust的标准库中有个`std::cell`模块，通过共享的可变容器允许以受控的方式进行可变性。

### Cell和RefCell

`std::cell`模块中的`Cell<T>`和`RefCell<T>`是两个提供内部可变性的共享可变容器。

> 术语：内部可变性（Interior Mutability）如果某个类型的内部状态可以通过对它的共享引用来更改，则它具有内部可变性。

通过`Cell<T>`的源码可知，只有实现了`Copy`的类型`T`，才可以使用`get`方法获取值；但任何类型`T`都可以使用`set`方法修改值。`get()`方法，返回所包含值的复制。`set()`方法，设置所包含的值。

使用`Cell<T>`及其提供的`get/set`方法，实现结构体内字段可变的示例：

```rust
use std::cell::Cell;

struct SomeStruct {
    regular_field: u8,
    special_field: Cell<u8>,
}

fn main() {
    let my_struct = SomeStruct {
        regular_field: 0,
        special_field: Cell::new(1),
    };

    let new_value = 100;
//    my_struct.regular_field = new_value; // ERROR: `my_struct`是不可变的

    my_struct.special_field.set(new_value); // WORKS: `special_field`是`Cell`类型的，它是可变的
    assert_eq!(my_struct.special_field.get(), new_value);
}
```

相对于`Cell<T>`而言，`RefCell<T>`可用于更普遍的情况。同时，相对于一般的静态借用，`RefCell<T>`具有动态借用检查机制，使得编译器不会在编译期，而是在运行时做借用检查。

`borrow()`方法，不可变借用被包裹值，可存在多个。`borrow_mut()`方法，可变借用被包裹值，只能有一个，且被借用时不能再可变借用。

示例如下：

```rust
use std::cell::RefCell;
use std::thread;

let result = thread::spawn(move || {
   let c = RefCell::new(5);
   let m = c.borrow();

   let b = c.borrow_mut(); // ERROR: 借用时不能再可变借用
}).join();

assert!(result.is_err());
use std::cell::RefCell;

fn main() {
    let c = RefCell::new(5);
    *c.borrow_mut() = 7;
    assert_eq!(7, *c.borrow());

    let x = RefCell::new(vec![1,2,3]);
    println!("{:?}", x.borrow());
    x.borrow_mut().push(4);
    println!("{:?}", x.borrow());
}
```

`Cell<T>`和`RefCell<T>`小结：

- `Cell<T>`适用于实现了`Copy`的类型（复制语义），`RefCell<T>`适用于未实现`Copy`的类型（移动语义）。
- `Cell<T>`使用`get/set`方法操作值，`RefCell<T>`使用`borrow/borrow_mut`方法获取引用进而再操作值。

### UnsafeCell

`std::cell::UnsafeCell`，Rust内部可变性的核心原语。`Cell<T>`和`RefCell<T>`的内部可变性是通过`UnsafeCell<T>`来包装他们的内部数据。`UnsafeCell<T>`类型是通过共享引用持有可变数据的唯一合法方式。源码如下：

```rust
#[lang = "unsafe_cell"]
#[stable(feature = "rust1", since = "1.0.0")]
#[repr(transparent)]
pub struct UnsafeCell<T: ?Sized> {
    value: T,
}

pub const fn get(&self) -> *mut T {
    self as *const UnsafeCell<T> as *const T as *mut T
}  
```

其`get()`方法，将一个值的不可变引用（`＆self`），强制转换三次：首先避免共享引用（`*const UnsafeCell<T>`），其次是不变原生指针（`*const T`），然后是可变原生指针（`*mut T`），最后将其返回给调用者。

### 结语

Rust中的可变或不可变主要是针对一个变量绑定而言的。对于类型而言，Rust标准库中的`std::cell`模块（`Cell`, `RefCell`等），提供内部可变性的容器，弥补了Rust所有权机制在灵活性上和某些场景下的不足。

- 通常情况下，共享不可变，可变不共享。
- 内部可变性，单线程使用`Cell <T>`和`RefCell <T>`。
- 内部可变性，多线程使用`Mutex<T>`，`RwLock<T>`。

### 更多链接

- [OnceCell](https://github.com/matklad/once_cell) - single assignment cells and lazy statics without macros

> 原文：https://rustcc.cn/article?id=37d1cb4f-5cc9-4adc-b41a-dbe4914bf4b5