---
title: "Rust初体验"
date: 2024-09-27
author: 马加奥
tags: ["Rust"]
categories: ["后端", "Rust"]
---

Rust是一种系统编程语言，专注于安全性、速度和并发性。它由Mozilla开发，旨在解决传统系统编程语言中的一些常见问题，尤其是内存管理和并发编程中的潜在安全隐患。
Rust被广泛应用于系统级编程，如操作系统内核开发、嵌入式开发、高性能Web服务等，同时也因为其可靠性，在构建安全性要求高的应用中表现出色。
<!--more-->

## 一、从事故中诞生的语言

2006年，29岁的Graydon Hoare是Mozilla的一名程序员。有一天下班回家，突然发现家中电梯因为软件崩溃没法运行。这已经不是第一次了，但Hoare家恰好住在21楼。

他一边爬楼梯一边暗自恼火，「这太可笑了，我们这些搞计算机的人，甚至都没法造出一个不崩溃的电梯！」

作为程序员，Hoare很清楚问题所在：电梯等设备内部的软件通常都是用C或C++编写的，好处在于运行速度快，但也很容易意外引入内存错误，造成程序崩溃。

或许是被愤怒的情绪激起了创造力，爬完楼梯回到家中后，Hoare打开电脑，开始设计一种新的编程语言。

Hoare设理想中的编程语言够编写出`简洁`、`短小`但`运行速度快`的代码，而且能从根本上杜绝内存错误。

Rust的雏形孕育而生；

Rust从一场电梯事故中诞生，经过Graydon Hoare孜孜不倦的投入精力，Rust最终被Mozilla重视，Mozilla意识到，Rust可以帮助他们构建出更好的浏览器引擎。在浏览器这种复杂软件中，有很多场景出现危险的内存错误。看上去似乎Rust可以避免这个问题。于是Mozilla开始向Rust投入人力支持。最终在2015年Rust正式问世；

## 二、什么是Rust

Rust是一种系统编程语言，专注于安全性、速度和并发性。它由Mozilla开发，旨在解决传统系统编程语言中的一些常见问题，尤其是内存管理和并发编程中的潜在安全隐患。

- **内存安全**：Rust具有非常严格的内存管理机制，通过其`所有权模型`，自动管理内存的分配和释放，从而避免了常见的内存泄漏、悬垂指针等问题，最终实现Rust不依赖GC，便可以准确进行内存管理的能力。
- **并发编程**：Rust通过其借用检查器和`所有权模型`，帮助程序员编写出安全且无数据竞争的并发代码。
- **性能**：Rust编译后的二进制文件执行效率非常高，接近C和C++，因此适合需要高性能的应用，如操作系统、游戏引擎、嵌入式系统、高性能Web服务器等。
- **生态系统**：Rust有一个活跃的开发者社区和丰富的第三方库，并且其包管理器Cargo使得依赖管理和构建过程非常便捷。

Rust被广泛应用于系统级编程，如操作系统内核开发、嵌入式开发、高性能Web服务等，同时也因为其可靠性，在构建安全性要求高的应用中表现出色。

## 三、Rust的未来

在[TIOBE编程社区的编程语言排行榜](https://www.tiobe.com/tiobe-index/)中，Rust排列第14位，看上去还是与Python、Go、Java、Javascript等高度商业化的语言相差甚远。我们不得不承认，Rust还是一个较为小众的编程语言，但不可忽略的是，Rust的发展速度非常迅猛。

实际上，围绕着 Rust 的描述有很多，有人说它没有历史包袱，能够将表达力、高性能、内存安全集于一身，甚至说掌握了 Rust 就掌握了许多语言精髓。这种说法不无道理，因为现在有人用 Rust 替代 Python 去写机器学习相关的应用，有人拿它写前端 UI，有人用它去实现区块链，有人拿它重建技术栈，甚至于 Linux 官方还接受 Rust 作为除 C 语言之外唯一可以进行内核开发的语言。

[美国国防部建议使用 AI 自动将 C 代码转换为 Rust - OSCHINA - 中文开源技术交流社区](https://www.oschina.net/news/305418/darpa-c-to-rust)

但Rust的学习曲线也非常陡峭，特殊的开发思想很容易在语法阶段就劝退大多数的初学者，但是不要害怕，接下来，我们一起尝试了解Rust的基础使用吧；

## 四、基础使用

学习一门语言最好的方式就是用现有熟悉的语言与其进行对比，我们作为前端工程师，可以尝试从TypeScript的角度来学习Rust；

### 4.1 **Cargo**

在前端的生态中，我们有Npm、Yarn、Pnpm等多种包管理器，我们可以通过npm init 等方式快速创建一个前端项目，也可以通过npm install 或者update管理项目的模块依赖；在Rust生态中，Cargo则类似Npm的角色；

Cargo 是Rust 的官方构建系统和包管理器，我们可以通过快速创建一个Rust项目，

```rust
cargo create name
```

或者创建一个对外的包项目crate：

```rust
cargo create name --lib
```

而Rust的模块文件也与package.json类似，包含着项目的信息，以及依赖的外部crate等等；

```yaml
[package]
name = "fibonacci"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib", "rlib"]

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
wasm-bindgen = "0.2.93"
```

外部crate维护在crates.io包服务器中；

[crates.io: Rust Package Registry](https://crates.io/)

### 4.2 基础语法

- 变量的可变性与不可变性

```rust
fn main() {
    let hello_str = String::from("hello world");
    println!("Let’s say: {}", hello_str);
    hello_str = String::from("hello rust");
    println!("Let’s say: {}", hello_str);
}
```

上面列举的代码，从我们使用JavaScript的思维来看，是很常规的变量更新操作，但是在Rust中，let 申请分配的变量与const是类似的，不可变的。当我们期望一个可变的Rust变量时，需要显式创建：

```rust
fn main() {
    let mut hello_str = String::from("hello world");
    println!("Let’s say: {}", hello_str);
    hello_str = String::from("hello rust");
    println!("Let’s say: {}", hello_str);
}
```

而const与JavaScript中的语法相似，常用于常量的声明，而既然是常量，则很明显无法使用mut，并且需要显式标注类型；

```rust
const ONE_HOURS_IN_SECONDS: u32 = 60 * 60 * 1;
```

- 数据类型

Rust也是一个强类型语言，因此不同于JavaScript，Rust与Java、Go一致，拥有严格的数据类型。

各个强类型的语言的数据类型大致相似：例如整型、浮点型、布尔型、字符型，元组、数组， char等等；

- 结构体

struct，或者 structure，是一个自定义数据类型，允许你包装和命名多个相关的值，从而形成一个有意义的组合。如果你熟悉一门面向对象语言，struct 就像对象中的数据属性；结构体与TypeScript的语法不太相同；

```rust
struct Student {
    grade: u8;
    name:  String;
    stu_id: u64;
}

let student = Student {
    grade: 1,
    name: "Alice".to_string(),
    stu_id: 2021000000001,
};

let mut student = Student {
    grade: 1,
    name: "majiaao".to_string(),
    stu_id: 2021000000001,
};
student.name = String::from("mario");
```

我们可以为`Student` 声明一些结构体方法体：

```rust
impl Student {
    fn get_grade(&self) -> u8 {
        self.grade
    }
}
// 
let grade = student.get_grade();
println!("student grade is: {}", grade);
```

- 使用包，crate，模块管理规模不断增长的代码

当你编写大型程序时，组织你的代码显得尤为重要。通过对相关功能进行分组和划分不同功能的代码，你可以清楚在哪里可以找到实现了特定功能的代码，以及在哪里可以改变一个功能的工作方式。

在 Rust 中，模块化是通过 `mod` 关键字来实现的，用来组织代码并提供逻辑分离。模块可以帮助管理大型代码库，并且可以通过子模块、私有性控制等机制组织代码。下面是一个基础的模块声明：

```rust
mod my_module {
    pub fn greet() {
        println!("Hello from my_module!");
    }
}

fn main() {
    my_module::greet();
}
```

上面只是一个简单的内置的模块，真正在项目中，我们通常把不同的功能模块通过文件的方式隔离：

```rust
rust_project
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

```rust
// 创建快捷引用方式
use crate::garden::vegetables::Asparagus;
// 引用声明
pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

```rust
pub mod vegetables;
```

Rust编译器会在项目的 `src` 目录中查找名为 `vegetables.rs` 或`vegetables/mod.rs`的文件。如果找到，它会将该文件内容作为模块的一部分。

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

### 4.3 多线程编程

因为Rust所有权规则的存在，因此在`多线程的场景`下，不需要考虑竞态的场景。Rust创建一个线程非常简单：

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {i} from the spawned thread!");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {i} from the main thread!");
        thread::sleep(Duration::from_millis(1));
    }
}
```

当我们需要不同的线程间进行通讯时，Rust的设计思想与Golang等现代语言有异曲同工之处，即：「不要通过共享内存来通讯；而是通过通讯来共享内存。」

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("received: {received}", received);
}
```

通讯共享内存的方式要稳定于共享内存式通讯，例如Java，虽然有状态锁等等来实现线程同步，但在复杂的并发场景下，还是可能会发生死锁、数据竞争；

### 4.4 生命周期

来不及分享了，先挖个坑吧。

## 五、Rust的不同之处

### 5.1 所有权

在上面我们大致了解了Rust语言的基础语法等，接下来我们对Rust进一步进行了解，接触到Rust的第一个重要的、与众不同的特性：所有权；

说到Rust的所有权策略，就不得不提到程序执行时的内存管理。在我们的常规认知中，程序内存管理主要有以下两种方式：

1. 一些语言中具有垃圾回收机制，在程序运行时有规律地寻找不再使用的内存；如JavaScript、Java JVM、Python、Golang等等；
2. 一些语言中，程序员必须亲自分配和释放内存。例如C语言通过malloc动态分配内存后，需要free手动释放内存；

而Rust则另辟蹊径，通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。如果违反了任何这些规则，程序都不能编译。在运行时，所有权系统的任何功能都不会减慢程序。如果理解了所有权，我们将有一个坚实的基础来理解那些使 Rust 独特的功能；

首先，我们在JavaScript，对于内存的存储位置并不用过度考虑，但我们都知道，代码运行内存分配主要有栈和堆，跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间，这些问题正是所有权系统要处理的。

首先，让我们看一下所有权的规则：

> Rust 中的每一个值都有一个 **所有者**（*owner*）。
值在任一时刻有且只有一个所有者。
当所有者（变量）离开作用域，这个值将被丢弃。
> 
- 作用域

```rust
   {                      
        let s = "hello";

        // 使用 s
   } // 此作用域已结束，s 不再有效                      
```

当变量离开作用域后，Rust 自动调用drop函数并清理变量的堆内存

- 移动

从JavaScript的开发经验来看，这是一个很简单的代码片段，预期也是会正常输出s1、s2内容。然而回顾一下所有权的规则：「值在任一时刻有且只有一个所有者」存储在同一个堆内存地址上的数据，无法同时存在两个所有者，因此所有权需要进行交接：s1 → s2； s1 drop；

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    println!("x = {}, y = {}", s1, s2);
}
```

### 5.2 借用

看上上面的例子，大家可能想要吐槽，移动的存在可能会让我们开发支离破碎，但其实不会，和C类、Golang语言类似，Rust同样存在内存引用，让我们看接下来的例子：

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

我们把创建一个引用的行为称之为`借用` ；

上面的代码中，我们通过引用获取到了内存数据，那是否可以修改呢？让我们继续尝试下？

```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```

很遗憾，rust编译器报错了，引用不允许修改引用的值。不慌，我们可以创建可变引用；

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

太好了，有了上面的这些方法，我们可以像使用JavaScript一样开发Rust！但别急，Rust是多线程语言，为了避免竞态问题，还有需要注意的：

❌ 如果你有一个对该变量的可变引用，你就不能再创建对该变量的引用

```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);
```

✅

```rust
 let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

let r2 = &mut s;
```

❌  Rust不可以同时使用可变与不可变引用

```rust
let mut s = String::from("hello");

let r1 = &s; // 没问题
let r2 = &s; // 没问题
let r3 = &mut s; // 大问题

println!("{}, {}, and {}", r1, r2, r3);
```

✅

```rust
let mut s = String::from("hello");

let r1 = &s; // 没问题
let r2 = &s; // 没问题
println!("{} and {}", r1, r2);
// 此位置之后 r1 和 r2 不再使用

let r3 = &mut s; // 没问题
println!("{}", r3);
```

❌ 垂直引用

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");
    &s
}
```

因为 `s` 是在 `dangle` 函数内创建的，当 `dangle` 的代码执行完毕后，`s` 将被释放。不过我们尝试返回它的引用。这意味着这个引用会指向一个无效的 `String`，Rust 不会允许我们这么做。

✅ 解决！

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    s
}
```

总结一下：

- 在任意给定时间，**要么** 只能有一个可变引用，**要么** 只能有多个不可变引用。
- 引用必须总是有效的。

## 六、Rust&前端

### 6.1 为什么Rust在前端领域如此活跃

大家可能都有听说，前端生态中，esbuild、vite等核心模块，都在尝试使用Rust；

### 6.2 webAssembly

![image.png](static/image.png)

`WebAssembly`是一种新的编码方式，可以在现代的网络浏览器中运行 － 它是一种低级的类汇编语言，具有紧凑的二进制格式，可以接近原生的性能运行，并为诸如C、C++等语言提供一个编译目标，以便它们可以在Web上运行。它也被设计为可以与JavaScript共存，允许两者一起工作。

对于网络平台而言，WebAssembly具有巨大的意义——它提供了一条途径，以使得以各种语言编写的代码都可以以接近原生的速度在Web中运行。在这种情况下，以前无法以此方式运行的客户端软件都将可以运行在Web中。

WebAssembly被设计为可以和JavaScript一起协同工作——通过使用WebAssembly的JavaScript API，你可以把WebAssembly模块加载到一个JavaScript应用中并且在两者之间共享功能。这允许你在同一个应用中利用WebAssembly的性能和威力以及JavaScript的表达力和灵活性，即使你可能并不知道如何编写WebAssembly代码。

可以有各种语言通过编译的方式，生成webAssembly代码。

1. `C/C++`代码可以通过`Emscripten`工具链编译为`wasm`二进制文件，进而导入网页中供`js`调用；
2. `Rust`语言更是内置了对`WebAssembly`的支持；

**webAssembly解决了什么问题？**

在目前的`Web`应用中，`JavaScript`属于**一家独大**的地位。但是，由于`JS`是**弱类型语言**，变量类型不是固定的，在**使用变量前需要判断其类型，无疑增加了运算的复杂度，降低了执行效率**。

让我们回顾一下，V8引擎是如何处理JavaScript代码的：

![image.png](static/image%201.png)

1. V8引擎无法直接识别我们的源代码；
2. 结构化代码片段,生成抽象语法树；
3. 根据抽象语法树和作用域生成字节码；
4. 字节码通过解释器最终交予浏览器环境执行；

尽管有JIT混合编译的方式执行，但作为弱类型语言，JavaScript的性能还是有天然的不足；

而WebAssembly则解决了JavaScript的性能问题，webAssembly挂载在window下，可以直接调用，而其兼容性也较为友好，

![image.png](static/image%202.png)

接下来，让我们使用Rust，编译出一段可以在浏览器中直接运行的wasm文件；

```bash
cargo new webassembly_demo --lib
```

```rust
#![allow(unused)]
fn main() {
    use wasm_bindgen::prelude::*;

    // Import the `window.alert` function from the Web.
    #[wasm_bindgen]
    extern "C" {
        fn alert(s: &str);
    }

    #[wasm_bindgen]
    pub fn greet(name: &str) {
        alert(&format!("Hello, {}!", name));
    }
}
```

## 七、总结

Rust的学习路线比较陡峭，目前版本也并非稳定，基建也没有C、JavaScript这些成熟语言丰富。但我们可以通过学习Rust接触到平时前端开发中很难接触到的计算机底层原理，本次分享只是抛砖引玉，大家有兴趣可以继续学习。