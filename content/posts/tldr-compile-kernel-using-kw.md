+++
title = "#TLDR - How to compile the Linux Kernel using KWorkflow"
description = "Micro-post teaching how to compile the Linux Kernel using Kworkflow"
date = 2022-06-14T09:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["TLDR"]
tags = ['gsoc', 'kernel', 'compile']
featuredImage = ""
+++

If you already have a `.config` file ready, one way to compile the Linux Kernel is to install [kw](https://github.com/kworkflow/kworkflow) and run:

```sh
$ kw build
```

The command is time and resource intensive, so it might slow down your system for some time. It might fail and ask you to install some dependencies. For more information read the [`kw build` documentation](https://kworkflow.org/man/features/build.html).

---

This is a micro-post about the Linux Kernel compilation process, its main purpose is to highlight and redirect to valuable resources.