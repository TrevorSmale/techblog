+++
author = "Hugo Authors"
title = "Actually completing VimTutor"
date = "2024-09-10"
description = "Finishing VimTutor has really changed my perspective of VIM as is much more useful and efficient than I had previously thought"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "Certification", "ProLUG Course", "Study Technique", "Vim", "ProLUG",
]
categories = [
    "Learning", "Projects",
]
series = ["Projects"]

+++

<!--more-->

### Intro

Vim is a program created by the late Bram Moolenaar as an improvement to an older program called Vi. It is a modal text editor, meaning there are different modes of usage — I'll get into that later. While strongly associated with UNIX systems, Vim is available on other platforms as well. It is used to efficiently create and modify files from a simple terminal, requiring very limited system resources. Vim is a foundational tool for mastering Linux, allowing one to navigate the directory structure and edit configuration files like no other. Furthermore, Vi and/or Vim come bundled with most minimal Linux distributions.

### Vim Tutor

Vim Tutor is an in-built interactive tutorial that comes pre-packaged with Vim, designed to bring novices up to speed with the various modes and functions of this nimble tool. I had hopped into Vim Tutor before but never felt the need to complete it, as it seemed long and complex. Recently, my teacher and mentor Scott C. mentioned that this would be one of the first lessons we would undertake as part of the ProLUG system administration course. Being a dedicated student, I decided to give it my full attention — and I’m glad I did.

### The Halfway Point

In previous attempts, I had reached the halfway point and felt confident I had gained enough information to edit files. Some functions are unintuitive at first, like using the `hjkl` keys for navigation instead of arrow keys. There are many jokes about the overload people feel when they first encounter Vim's conventions. Jokes like never being able to exit the program or forgetting one's own name after memorizing commands are common. Mastering Vim requires memorizing commands until they become second nature, which simply takes time. This is why I hadn't completed Vim Tutor before — there’s only so much one can absorb before needing a break. However, this time, I was able to make it through to the end, pleasantly surprised by the new techniques I had learned.

### Understanding the Modes

Modes are what make Vim a powerful tool. There are three modes in Vim: Normal, Insert, and Visual. 

- **Normal mode** is the default mode, where you navigate a document using motions. It protects the document from accidental modifications and allows for a broader range of single-key commands.
  
- **Insert mode** is where characters or strings are added — fairly self-explanatory.

- **Visual mode** allows you to make selections, much like clicking and dragging with a mouse to highlight text.

Although I had known about these modes, it once seemed improbable that I would ever memorize them well enough to use them reflexively. However, with practice, I have done just that. Combining motions and commands can lead to huge efficiency gains. For example, you can navigate to a specific line, delete the first three letters, and paste something at the end of the line with just a few keystrokes.

### What I Recently Picked Up

#### Stringing Commands

What did I actually learn that I will immediately employ? Well, I now have a solid grasp of most single-key commands, so I’ll start stringing commands and motions together, such as deleting three words and appending text to the end of a line. 

#### Global Search and Replace

Additionally, I had known about global search and replace, but I hadn’t really used it before since it’s only useful for certain file types. 

#### Buffers and Deleted Lines

I also learned that Vim’s buffer, much like a clipboard, stores strings for later use — and deleted lines are stored there as well. I’ll be more mindful of using the buffer going forward.

#### Running Shell Commands

Lastly, running shell commands from within Vim seemed inefficient to me before, since I could easily jump in and out of files. However, I now understand that it can speed things up.

### In Summary

Vim isn’t something you just pick up on a whim. It’s more like a martial art, where you practice movements until they become second nature. In a world dominated by graphical user interfaces, Vim can seem like a waste of time and effort. Yet, when you’re working under the hood of an operating system, especially on bare-bones UNIX servers, it becomes clear that if everything is a file and many configurations must be made, Vim is the right tool for the job.



