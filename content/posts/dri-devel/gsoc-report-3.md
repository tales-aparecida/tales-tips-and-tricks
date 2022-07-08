+++
title = "Submitting patches (with kunit tests)"
summary = "Let's learn together how to write, run and upstream unit tests for the Linux Kernel using KUnit."
date = 2022-06-18T12:00:00-03:00
draft = true
authors = ["Tales L. Aparecida"]
categories = ["DRI-Devel"]
tags = ['gsoc', 'kernel', 'kunit', 'git']
featuredImage = "/images/mail-boxes.jpg"
+++

In a system like the Linux Kernel, with thousands of files with an ever-growing number of target devices, unit tests can provide a safety net against regressions, which itself can help newcomers feel comfortable suggesting changes, knowing full-well they won't be breaking related code.
[KUnit](https://kunTit.dev/index.html) is one of the Linux Kernel unit testing frameworks, and in this post I'll walk you through the process of writing, running and trying to upstream unit tests using it.

<!-- more -->

## KUnit

KUnit follows the white-box testing approach, that is, the tests are written with knowledge of internal system functionality. KUnit runs in kernel space and is not restricted to things exposed to user-space, its tests are written in the C language and _assert_ that Kernel components work as expected, for example: file system, system calls, memory management, device drivers and so on. If you are interested in testing user APIs, [kselftest](https://docs.kernel.org/dev-tools/kselftest.html) is probably a better choice.

### Running Tests

Running is quite simple, you just need to have a compilable Kernel code in your workspace and then run `python ./tools/testing/kunit/kunit.py run`. This should compile and run the whole available test suite.

## Writing tests



# References

- 
- https://kunit.googlesource.com/kunit-website/
- https://www.kernel.org/doc/html/latest/dev-tools/kunit/index.html
- https://www.youtube.com/watch?v=Y_minEhZNm8&t=14820s KUnit: New Features and New Growth
- https://www.youtube.com/watch?v=iaK_wcL1ekY&t=10275s Differences in bug classes, bug tracking and bug impact
- https://www.youtube.com/watch?v=iaK_wcL1ekY&t=14470s Kernel testing frameworks
https://www.youtube.com/watch?v=o0YyUfHG4h4&list=TLPQMTQwNTIwMjKUqPlnlVUtHg&index=3
https://www.youtube.com/watch?v=01o2O72DqKQ&list=TLPQMTQwNTIwMjKUqPlnlVUtHg&index=2
https://www.youtube.com/watch?v=_H8848Ggo3w
- https://gitlab.collabora.com/tonyk/linux/-/tree/kunit_drm

https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/kbuild/makefiles.rst
- https://wiki.linuxquestions.org/wiki/How_to_build_and_install_your_own_Linux_kernel Quite through tutorial about compiling the Kernel for the first time.

- Prepare your code
- Prepare your commits
- Prepare your cover letter
- Prepare your email client
- Prepare your receivers list

https://kunit.dev/usage/index.html#submitting-tests-upstream

---

"Mail Boxes" by Gregory Jordan is licensed under CC BY-NC-ND 2.0. To view a copy of this license, visit https://creativecommons.org/licenses/by-nd-nc/2.0/jp/?ref=openverse.