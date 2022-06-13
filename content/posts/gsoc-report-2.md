+++
title = "Finding bugs using IGT and git bisect"
description = "How to combine IGT GPU Tools with Git bisect to report an issue in the AMDGPU driver"
date = 2022-06-12T12:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["Report"]
tags = ['gsoc', 'kernel', 'amdgpu', 'igt']
featuredImage = "/tales-tips-and-tricks/images/repair-bug.jpg"
+++

The first step to eliminate bugs is to find a way how to reproduce them consistently. _Wait... what?_

Test suites are great for that, since they can simulate very specific behavior in a timely manner. [IGT GPU Tools](https://gitlab.freedesktop.org/drm/igt-gpu-tools) is a collection of tools for development and testing of the DRM drivers, and, as such, it can help us to find and reproduce bugs.

I intend to help expand the AMDGPU tests' list in my the [GSoC project](https://summerofcode.withgoogle.com/proposals/details/TKAqZe03), so it made sense trying to run them right away. Cloned and built the IGT project then tried to run the "amdgpu" tests using a TTY in [text mode](https://ubuntuhandbook.org/index.php/2020/05/boot-ubuntu-20-04-command-console/):

```sh
$ ./scripts/run-tests.sh -t ".*amdgpu.*"
```

Unfortunately, the tests failed and _never came to a stop_. I tried interrupting the process using different techniques but, in the end, had to reboot pressing the Reset button. Looking through the partial results I found that one of the subtests of `amd_cs_nop` was causing the problem... But why weren't the test just failing or crashing?

In an attempt to debug what was happening inside the subtest I enabled the DRM debugging messages with:

```sh
echo 0x19F | sudo tee /sys/module/drm/parameters/debug
```

This [mask](https://01.org/linuxgraphics/gfx-docs/drm/gpu/drm-internals.html#c.drm_debug_category) activates all debugging logs but "verbose vblank" and "verbose atomic state". Unfortunately, again, this debugging logs only managed to show me, with my limited knowledge, that _something_ had been locked in an infinite loop.

{{< notice tip >}} Ask for help! {{< /notice >}}

After sharing my experience with my mentors, they assured me the problem was _probably_ caused by the kernel code itself, not with my setup or my compilation `.config`. So, to pinpoint what part of the code was causing the problem I could look through space (stepping throughout the calls with **gdb**) or time (using **git**) and I choose the latter.

So now the plan was to find when the bug was introduced. Luckily it wasn't so hard, a single `git checkout` to the previous release in the [`amd-staging-drm-next`](git@gitlab.freedesktop.org:agd5f/linux.git) branch, which was `v5.16`, only 1000 commits or so behind the HEAD of the branch. It might seem like a lot to go through, but our time-machine is quite fast: [`git bisect`](https://git-scm.com/docs/git-bisect).

According to Wikipedia, the [bisection method](https://en.wikipedia.org/wiki/Bisection_method) is a root-finding method that applies to any continuous function for which one knows two values with opposite signs. You probably already know it as binary-search. In this case, we are searching for the first commit in which the test fails, so to start our journey 

```bash
git bisect start
git bisect bad # HEAD
git bisect good v5.16
```

And with it, `git` informs us there will be around 10 steps and checks out to the middle of those commits. For each step, build, deploy, reboot into the new kernel and run tests. Quite tedious and time consuming. Right now, I'm not sure I could do it in a QEMU environment, but certainly, if I ever need to bisect the kernel in the future I'll look into it, because in that case I would be able to use `git bisect run`, which would allow the whole process to be automatized.

{{< notice tip >}} Always read the **build** result before **deploying**. {{< /notice >}}

In the end, the whole process took a full afternoon... just to find myself in a failed bisection. I had the misfortune of making some mistake in the middle of the road, probably skipping the _build_ step by accident due to a compiling error, and marked a commit with the wrong flag. After restarting the bisect in the following day, I made sure to avoid my mistake be renaming each build with the short hash of the commit I was compiling. And finally 🎉, found the first broken commit: https://gitlab.freedesktop.org/agd5f/linux/-/commit/e68efb27647f2106d6b545667f35b2ea39746b57

Well... at least, I found the commit where the `amd_cs_nop` started failing. It looks promising, given it handles a mutex lock, and my mentors think so as well. Next step was reporting the bug, by simply creating an issue following the "BUG template" given at https://gitlab.freedesktop.org/drm/amd/-/issues/:

{{< notice info >}} https://gitlab.freedesktop.org/drm/amd/-/issues/2048 {{< /notice >}}

And that's that. Thanks for reading. ❤️

---

"Repair Bug" by AZRainman is licensed under CC BY 2.0. 