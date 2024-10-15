+++
author = "Hugo Authors"
title = "GO CLI Utility"
date = "2024-10-15"
description = "The Process of creating a personal utility for Linux using GO and Cobra CLI"
draft = "false"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "GO", "Notes", "Research"
]
categories = [
    "Knowledge Exchange",
]
series = ["Teaching"]

+++

<!--more-->

# Making a Utility for Linux 🚀

Recently, I was chatting with fellow enthusiasts in the ProLUG group about creating system utilities with Go. 

Between learning about it on my own time and getting input from others, I realized there are so many compelling reasons for using Go as a utility language. 🛠️ 

Currently, the idea isn't as prevalent as Python or Bash, but it’s gaining traction. This makes sense since Go was designed with modern, interconnected systems in mind. 

## What is a Utility? 🤔

A system utility streamlines repetitive tasks by offering custom commands tailored to your workflow, boosting efficiency for system administrators and engineers. Work smarter, not harder. 💡

## CLI Tool 🖥️

Go offers a package called Cobra CLI [^1], which enables the creation of command-line interfaces that take input from the terminal and execute predetermined logic. I’m putting this simply in case you’re unfamiliar with how programs work 😆. 

The CLI is the simplest form of a computer program and is perfect for Linux system folks.

## Core Idea 🎯

The core idea of this article is to explain how a Go program can act as a personalized utility that lives on your system or can be deployed across many. Go compiles code into a binary—native computer language. Once built, the binary runs reliably without failure, assuming the program logic is solid. This makes Go ideal for utilities that are called repeatedly to perform simple tasks.

## Automate Everything 🤖

Managing systems involves a lot of repetitive tasks, and the goal of any sysadmin or engineer is to automate them. Tools like Ansible, Python, and Bash are already well-known for automation. However, there are specific reasons to use Go, which I’ll explore further in a future article titled *"Why Go?"* where I’ll break down why it belongs in your quiver of tools 🏹.

---

### CLI Programming Portion ⚙️

#### Setup
1. **Install** Go and Cobra CLI (if not already installed).
2. **Initialize** a new Go project with Cobra CLI.

#### CLI Logic
1. **Add commands and flags** to the Cobra application.
2. **Implement utility functions**, such as disk usage checks or file listings.

### Compilation and Installation 🏗️

#### Compile
1. **Build** the Go project into a single x86 Linux binary.
2. **Test** the binary locally to confirm functionality.

#### Install
1. **Move** the binary to `/usr/local/bin` for global access.
2. **Set permissions** on the binary to allow execution.

#### Add to PATH
1. **Check** if `/usr/local/bin` is in your PATH.
2. If not, **add it** to your `.bashrc` or `.bash_profile`.

### Testing Portion 🧪
1. **Run** the utility from any location to confirm it works.
2. Optionally, **add help documentation** or more commands to the CLI.

### Packaging 📦
1. **Provide installation instructions** if you plan to share it.
2. **Upload** the binary or source code to GitHub for distribution.

---

## Learning Resources

Ok this was just a simple outline you say. Yes, I am trying to convey information simply for sanities sake, this is how I process things anyway.

In order to make your own utility you will have to go off on your own learning adventure. Below I am giving you some resources for getting started.

### Getting up and running with GO

#### Brief Intro [^2]

#### For the impatient [^3]

There are many ways to get this done. For the clever and impatient, I suggest finding a pre-made CLI application that you can deconstruct. [^3]

#### For the Patient [^4]

For those who like to build a foundation of understanding, I suggest watching a lecture followed by a tutorial and finally doing the project. [^4]

## Conclusion

I try to keep things simple and succinct, so this is where I ride off into the sunset. Good luck and happy learning. 

---

## Footnotes ⛓️

[^1]: Cobra CLI Source Code [GitHub Repository](https://github.com/spf13/cobra) User: spf13, Current.

[^2]: GO in 100 Seconds [Youtube Video](https://www.youtube.com/watch?v=446E-r0rXHI) Channel: Fireship, 2021.

[^3]: Cobra CLI Samples [GitHub Repository](https://github.com/Adron/cobra-cli-samples) User: Adron, 2022.

[^4]: Golang Tutorial Series Playlist [Youtube Channel](https://www.youtube.com/watch?v=etSN4X_fCnM&list=PL4cUxeGkcC9gC88BEo9czgyS72A3doDeM) Channel: Net Ninja, 2021.

