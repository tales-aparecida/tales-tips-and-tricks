+++
title = "#TLDR - How to create a basic .config using KWorkflow"
summary = "Micro-post teaching how to create a basic .config using Kworkflow"
date = 2022-06-13T09:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["TLDR"]
tags = ['gsoc', 'kernel', 'compile']
featuredImage = ""
+++

One way to create a good enough `.config` file is to install [kw](https://github.com/kworkflow/kworkflow) and run:

```sh
$ kw configm --fetch --optimize
```

The command might ask if you want to overwrite the existing `.config` file. It might also ask to define some other settings which, in general, will work when choosing the default option. Read more about the command at the [`kw configm` documentation](https://kworkflow.org/man/features/configm.html).

---

This is a micro-post about the Linux Kernel compilation process, its main purpose is to highlight and redirect to valuable resources.