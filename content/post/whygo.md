+++
author = "Hugo Authors"
title = "Learning Regex"
date = "2024-10-03"
description = "Really Getting to know Regular Expressions"
draft = "true"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "Regex", "Notes", "Research"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Intro 

Recently I was speaking with fellow enthusiasts in the ProLUG group about creating system utils with GO.

Between learning about it on my own time and getting input from others, I realized there are so many compelling reasons for using GO as a utility language.

## Brief History

### Intro

Go, also known as Golang, is a statically-typed, compiled programming language created at Google in 2007. It was developed by Robert Griesemer, Rob Pike, and Ken Thompson, the latter two being instrumental in the development of Unix and the C programming language.

### Motivation

The language emerged from frustrations with existing tools used at Google. Developers struggled with slow compilation times, complex dependency management, and challenges handling concurrency, especially for large, interconnected systems.

### Philosophy

Go was built to offer simplicity, fast compilation, and robust support for concurrency through lightweight threads called goroutines. This makes it ideal for cloud computing, distributed systems, and large-scale networking applications.

### C of the 21st Century

Go has been described as the “C of the 21st century,” emphasizing its role as a low-level, efficient language with modern syntax. Just as C was pivotal for system-level programming in the past, Go fills a similar niche today, optimized for multi-core processors and web services.

## Reasons for using GO as a utility language

### 1. **It’s Compiled**

#### **What is Compilation?**  
Compilation means converting a program’s source code into a binary—a file that can run natively on a processor without needing an interpreter or external tools.

---

#### **Benefits of a Binary**  

#### 1. **Portability**  
- A binary is a self-contained, single file that can run on any compatible system without additional configuration or dependencies. This simplifies distribution and deployment, especially for large systems.

#### 2. **Integrity and Monitoring**  
- Since a compiled binary doesn’t change unless recompiled, its integrity can be easily verified. This allows monitoring tools to fingerprint its characteristics, ensuring the binary remains intact and un-tampered.

#### 3. **Resistance to Modification**  
- Unlike scripts or interpretable code, binaries are difficult to alter directly. Any changes require updating the original source code and recompiling the program, making them more secure against unauthorized modifications.

---

## Making a CLI Utility for Linux

### 1. Setup
1. Install Go and Cobra CLI (if not installed).
2. Set up a new Go project and initialize a Cobra CLI application.

### 2. Build the CLI Logic
1. Add commands and flags to the Cobra application.
2. Implement simple utility functions (e.g., disk usage check, file listing).

### 3. Compile the Application
1. Build the Go project into a single x86 Linux binary.
2. Test the binary locally to ensure it works.

### 4. Install the Binary
1. Move the binary to `/usr/local/bin` for global access.
2. Set appropriate permissions on the binary.

### 5. Add to PATH
1. Verify if `/usr/local/bin` is in your PATH.
2. Add it to the PATH in `.bashrc` or `.bash_profile` if not already present.

### 6. Test the CLI Utility
1. Run the utility from any location to confirm it works.
2. Optionally, add help documentation or more commands to the CLI.

### 7. (Optional) Package and Share
1. Provide installation instructions for others (if needed).
2. Upload the binary or code to GitHub for distribution.





### ed

### grep

### sed

### awk

### Perl

Known as the Swiss army knife of scripting languages"

### Broader UNIX tools

### Standardization

---

### Helpful Links ⛓️


[^1]: Example [Article](website) Publisher, 2024.


