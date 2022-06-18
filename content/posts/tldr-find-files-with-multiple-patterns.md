+++
title = "#TLDR - How to search for files containing two or more strings"
summary = "Micro-post teaching one way to find files matching multiple patterns chaining grep invocations."
date = 2022-06-18T16:40:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["TLDR"]
tags = ['gsoc', 'gnu/linux', 'linux', 'bash', 'shell']
featuredImage = ""
+++

You might already know `grep`, the program to find and return lines matching a given pattern, but what if you want to find files matching multiple patterns simultaneously? It's simple, just chain searches, i.e find files matching the first pattern, then find files matching another pattern within the resulting list of the first search, and so on:

```sh
$ grep -lr "first pattern" folder/to/search/* | grep -l "another pattern" `cat -` | grep -l "and so on..." `cat -`
```

Using a real example; let's search, in the Linux kernel folder, for `.c` and `.h` files importing `"kunit/test.h"` than get their lines containing the word "module" (case-insensitive):

```sh
$ grep -lr --include "*.[ch]" 'kunit/test.h' ./linux | grep -i 'module' `cat -`
```

{{< notice info >}}
- `grep`: command
- `-l`, `--files-with-matches`: Suppress normal output; instead print the name of each file matching the pattern.
- `-r`, `--recursive`: Read all files under each directory, recursively.
- `--include=GLOB`: Search only files whose base name matches GLOB using wildcard matching; here, files with the extensions ".c" or ".h".
- `'kunit/test.h'`: find files with this string
- `./linux`: Start at "linux" folder under the current working directory.
- `| grep`: pipe the output of the first command (list of filenames) to the next search
- `-i`, `--ignore-case`: Use case-insensitive pattern matching.
- `'module'`: this time, search for the string "module"
- `` `cat -` ``: search the files given by the standard input, in this case the piped result from the first `grep`.
{{< /notice >}}
 
This should run almost instantaneously, because `grep` is powered by _magic_.

FYI: I've used this command to find out whether any Kunit tests were being exported as a module. Turns out, no, at least not that I could find.

---

This is a micro-post about the GNU/Linux environment, its main purpose is to highlight and redirect to valuable resources.