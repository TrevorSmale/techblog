+++
author = "Hugo Authors"
title = "Embedded Rust? 🦀"
date = "2025-01-12"
description = "What I learned about Rust on Embedded Hardware and why some features greatly improve the development experience."
tags = [
  "Tech", "Rust", "Embedded", "Development"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Yes

Firstly Rust on Embedded is a different beast as the standard library is not used and memory safety is not on by default. However, there are still some advantages over a popular language like C or C++. **HAL** or Hardware Abstraction Layer separates the hardware from the code enabling more portable software that can compile to multiple architectures. Cargo improves development ergonomic by creating and managing the project and its dependancies. Thirdly, the build system is unified across platforms, so code will compile on Windows, Mac and Linux in the same way.
Rust on embedded systems is a different challenge, as it does not use the standard library, and memory safety is not enabled by default. However, it still offers several advantages over popular languages like C or C++.
One key benefit is the Hardware Abstraction Layer (HAL), which separates hardware-specific details from the code, enabling more portable software that can compile across multiple architectures. Additionally, Cargo enhances development ergonomics by simplifying project and dependency management.
Lastly, Rust’s unified build system ensures consistent behavior across platforms, allowing code to compile seamlessly on Windows, macOS, and Linux.

## HAL 🪢

Is really interesting, it is the concept of mapping hardware and storing the map for API access.
HAL leverages Peripheral Access Crates (PACs), which are auto-generated Rust crates representing the registers and bitfields of a microcontroller. PACs allow safe and direct access to hardware registers while ensuring Rust’s strict type-checking and ownership rules are followed. HAL sits on top of PACs, abstracting these low-level details.
Rust embedded HALs adhere to the embedded-hal traits—a collection of interfaces defining common operations like GPIO pin control, SPI/I2C communication, timers, and ADC usage. By implementing these traits, HAL provides a uniform way to interact with hardware, regardless of the underlying platform.
HAL abstracts device-specific features into a user-friendly API. For example:

- Configuring a GPIO pin involves selecting its mode (input, output, pull-up, etc.) without directly modifying hardware registers.
- Communication protocols like SPI or I2C are exposed through easy-to-use Rust methods (read, write, transfer, etc.).

## Cargo 📦

Cargo handles dependencies seamlessly using Cargo.toml. Developers specify libraries (called “crates”) with version constraints, and Cargo fetches and builds them automatically.
Cargo:

- Ensures reproducible builds by generating a Cargo.lock file that locks dependency versions.
- Community-driven ecosystem (e.g., crates.io) simplifies finding and using high-quality, maintained libraries.

### Managing Dependancies ⚙️ with Cargo.toml

```toml
[dependencies]
embedded-hal = "0.2.6"
stm32f4xx-hal = "0.14"
```

### Cross-compilation support is integrated via targets 🎯

```bash
cargo build --target thumbv7em-none-eabihf
```

### Enforced project Structure 👮‍♂️

```bash
my_project/
├── Cargo.toml       # Dependencies & configuration
└── src/
    └── main.rs      # Application entry point
```

## Cross Platform 💻 💼

- Tools like probe-rs allow seamless debugging and flashing of embedded devices on multiple platforms (Linux, macOS, Windows).
- The cargo ecosystem integrates testing, building, and dependency management across platforms without additional tools.

## Experienced Embedded Devs are switching

I was first made aware of Rust for embedded by experienced devs on Youtube who proclaimed their love of rust over C for professional projects. Channels like:
https://www.youtube.com/@therustybits
https://www.youtube.com/@JaJakubYT
https://www.youtube.com/@floodplainnl
https://www.youtube.com/@embedded-rust

are all claiming that they have switched over and are not looking back.

## Personal Development

I’m writing this post as I plan to develop hardware projects using Rust for embedded systems. The combination of Rust and RISC-V microcontrollers is a particularly exciting intersection. In my sights are the ESP32-C3 and Raspberry Pi Pico 2, both of which I’m considering for upcoming projects. Instead of dealing with messy C, slow MicroPython, or the limitations of TinyGo, Rust allows me to create clean and performant projects—something I always strive for. Stay tuned for more updates!