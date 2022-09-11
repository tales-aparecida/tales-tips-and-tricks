+++
title = "GSoC 2022 Final Report"
summary = "What was proposed and achieved"
date = 2022-09-11T18:15:00+01:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["DRI-Devel"]
tags = ['gsoc', 'kernel']
featuredImage = "/tales-tips-and-tricks/images/applying-patch-to-belly.jpg"
+++

So here we have it, the end of my Google Summer of Code journey.
A few more than a hundred days have passed, and I can already tell that the
seeds have been sown for me to keep collaborating with open source software
from here on out.

<!-- more -->

My project’s primary goal was to create unit tests using KUnit for the AMDGPU
driver focusing on code used by GPUs from the same generation of the GPU
“RX 580” (DCE 11.2). We predicted that KUnit would have some limitations in
regards to testing GPU’s drivers, so we expected to see some collaboration in
that sense. Finally, we knew that I would be working in parallel with people
writing tests for newer generations of GPUs (DCN). I planned to keep track of
my weekly progress in my blog, trying to create an introductory material that
could help future newcomers.

For starters, this project was completely different from what I had in mind,
given that it was far from an individual experience with the Linux Kernel community;
it was actually a team effort to introduce unit testing to the AMD display driver
in a way that would encourage the community to spread KUnit into other GPU drivers.

Other surprise was how the unit tests didn't really interact with the GPU,
so all our worries about needing to mock devices, and intentions of writing
about it, were put aside.

In retrospect, there's still a lot to learn about the AMD codebase and graphics
stack in general, but there are also many things that I've learned and didn't
manage to share yet, about IGT, KUnit, and the email-patch workflow, which I'll
be writing in my personal blog in addition to the pair of posts there:

* [Reviewing patches](https://tales-aparecida.github.io/tales-tips-and-tricks/posts/dri-devel/gsoc-report-4/)
* [Finding bugs using IGT and git bisect](https://tales-aparecida.github.io/tales-tips-and-tricks/posts/dri-devel/gsoc-report-2/)

I've also dedicated some time reviewing the posts from my colleagues as well, like:

- [Isabella's post about graphics' stack](https://gitlab.com/flusp/flusp.gitlab.io/-/merge_requests/102)
- [Maíra's post about symbols](https://gitlab.com/flusp/flusp.gitlab.io/-/merge_requests/115)
- [Magali's post about rendering test coverage](https://gitlab.com/flusp/flusp.gitlab.io/-/merge_requests/119)

# Introducing Kunit test into AMDGPU

Our patch series is still under review:

* [drm/amd/display: Introduce KUnit to Display Mode Library](https://lore.kernel.org/dri-devel/20220831172239.344446-1-mairacanal@riseup.net/)

We have refactored this series many times before sending it, worrying about
how the code should be organized and how the tests should be compiled.
My biggest lesson was learning to *let go*. Do your best to send great patches
and reviews to the community, but don't let the fear of being slightly wrong
hold you back from pressing the submit button.

> Done is better than perfect!

I didn't manage to write as many tests as I planned to the DCE11.2 functions,
but for what it's worth, I'm making sure that the KUnit documentation is up to
date, so that anyone can write their own tests.

| Patch | Status |
| ----- | ------ |
| [drm/amd/display: Introduce KUnit tests for fixed31_32 library](https://lore.kernel.org/amd-gfx/20220831172239.344446-2-mairacanal@riseup.net/) | Under review |

* Co-developed: [drm/amd/display: Introduce KUnit tests to the bw_fixed library](https://lore.kernel.org/amd-gfx/20220831172239.344446-3-mairacanal@riseup.net/)

# Contributing to FLOSS projects

In my way to introducing unit tests to the AMD display driver I've managed to leave
some impressions behind, writing small patches and reviewing code, listed in
detail on the following sections.

## KWorkflow

[Kworflow](https://github.com/kworkflow/kworkflow) is a set of bash scripts that
helped me to compile and deploy the kernel in my testing system,
specially valuable to bisect code. In the beginning of my journey,
shortly before the community bounding period, I sent a couple of patches to
KWorkflow and reviewed a Pull Request.

| Patch | Status |
| ----- | ------ |
| [#592 Add support for GPUs identified as "Display controller" in kw device](https://github.com/kworkflow/kworkflow/pull/592) | Accepted |
| [#607 Enhance docs for kw-pomodoro and kw-report](https://github.com/kworkflow/kworkflow/pull/607) | Accepted |

- Reviewed [#606 Fix large initramfs issue](https://github.com/kworkflow/kworkflow/pull/606)

## IGT

[IGT GPU Tools](https://gitlab.freedesktop.org/drm/igt-gpu-tools) is a collection
of tools for development and testing of the DRM drivers. One of my first tasks
was to run the AMDGPU test suite using the GPU "RX580". After checking with the
community that my testing setup was correct, I reported a bug to the AMD issue
tracker, with proper bisection, and followed it through the patching process:

| Patch | Status |
| ----- | ------ |
| [lib/igt_kmod: fix trivial typos](https://lists.freedesktop.org/archives/igt-dev/2022-August/044675.html) | Under review |
| [CONTRIBUTING: Add reference for GTKDoc](https://lists.freedesktop.org/archives/igt-dev/2022-August/044674.html) | Under review |
| [lib/kselftests: Skip kselftest when opening kmsg fails](https://lists.freedesktop.org/archives/igt-dev/2022-August/044676.html) | Under review |
| [lib/igt_kmod: add igt_kselftests documentation](https://lists.freedesktop.org/archives/igt-dev/2022-August/044677.html) | Under review |

* Reported: ["RX 580: igt@amdgpu/amd_cs_nop@fork-compute"](https://gitlab.freedesktop.org/drm/amd/-/issues/2048)
* Tested: [Revert "drm/amdgpu: remove ctx->lock"](https://lore.kernel.org/amd-gfx/20220621144227.664800-1-luben.tuikov@amd.com/)
* Tested: [drm/amdgpu: Protect the amdgpu_bo_list list with a mutex v2](https://gitlab.freedesktop.org/agd5f/linux/-/commit/90af0ca047f3049c4b46e902f432ad6ef1e2ded6)
* [Closed the Issue](https://gitlab.freedesktop.org/drm/amd/-/issues/2048#note_0983d9d9b8474d04219aa07a88b12f594baa3be5).

## Linux Kernel - KUnit

[KUnit](https://docs.kernel.org/dev-tools/kunit/) is the Kernel Unit Testing Framework.
It not only brings a way to facilitate writing tests to the kernel,
but also running them using User Mode Linux or QEMU.
For most of time in the project we were discussing how to organize tests in the AMD folders,
and there were a lot of lessons from that, which I intend to share.
I have a lot of documentation patches in their way, some may be worth squashing,
but nonetheless, I have the intention to help making the KUnit documentation as clear as possible!

| Patch | Status |
| ----- | ------ |
| [Documentation: kunit: fix trivial typo](https://lore.kernel.org/linux-kselftest/20220813042055.136832-2-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: Kunit: Fix inconsistent titles](https://lore.kernel.org/linux-kselftest/20220813042055.136832-3-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: Fix non-uml anchor](https://lore.kernel.org/linux-kselftest/20220813042055.136832-4-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: Kunit: Add ref for other kinds of tests](https://lore.kernel.org/linux-kselftest/20220813042055.136832-5-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: remove duplicated docs for kunit_tool](https://lore.kernel.org/linux-kselftest/20220822022646.98581-2-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: avoid repeating "kunit.py run" in start.rst](https://lore.kernel.org/linux-kselftest/20220822022646.98581-3-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: add note about mrproper in start.rst](https://lore.kernel.org/linux-kselftest/20220822022646.98581-4-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: Reword start guide for selecting tests](https://lore.kernel.org/linux-kselftest/20220822022646.98581-5-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: add intro to the getting-started page](https://lore.kernel.org/linux-kselftest/20220822022646.98581-6-tales.aparecida@gmail.com/) | Accepted |
| [Documentation: KUnit: update links in the index page](https://lore.kernel.org/linux-kselftest/20220822022646.98581-7-tales.aparecida@gmail.com/) | Accepted |
| [lib: overflow: update reference to kunit-tool](https://lore.kernel.org/linux-kselftest/20220822022646.98581-8-tales.aparecida@gmail.com/) | Accepted |
| [lib: stackinit: update reference to kunit-tool](https://lore.kernel.org/linux-kselftest/20220822022646.98581-9-tales.aparecida@gmail.com/) | Accepted |
| [kunit: tool: fix --qemu_config help text](https://lore.kernel.org/linux-kselftest/20220819020815.183766-1-tales.aparecida@gmail.com/) | Under review |

* I started and intend to keep exploring a thread about ["running kunit tests on platform devices"](https://lore.kernel.org/linux-kselftest/443632b6-c589-ef62-2385-3e8406680343@gmail.com/). I believe there's a workaround using one of the approaches we learned while writing tests for AMDGPU.

## Linux Kernel - DRM

The Direct Rendering Manager (DRM) is a subsystem of the Linux kernel
responsible for interfacing with GPUs. There's still a lot to learn and contribute
to this subsystem, and I hope to discuss about introducing unit tests to other
folders during the Linux Plumbers Conference (more about that in a following section).
I only have two patches to VKMS to show, sent shortly before the community bounding period.

| Patch | Status |
| ----- | ------ |
| [drm/vkms: check plane_composer->map[0] before using it](https://lore.kernel.org/dri-devel/20220415111300.61013-2-tales.aparecida@gmail.com/) | Accepted |
| [drm/vkms: return early if compose_plane fails](https://lore.kernel.org/dri-devel/20220415111300.61013-3-tales.aparecida@gmail.com/) | Discarded |

* Tested: [KUnit tests for RGB565 conversion](https://lore.kernel.org/dri-devel/CAGVoLp47kQTuMJWVGtY-KMPf=opv3ted7MkbooEbdb2UWZqevg@mail.gmail.com/). Following this thread I've learned how to run kunit tests for other architectures, like PowerPC.

## Linux Kernel - AMDGPU

Inside the DRM subsystem resides the AMD folder, where you can find drivers for many generations of AMD GPUs.
Besides our main patch series, I've sent some minor patches and reviewed others.

| Patch | Status |
| ----- | ------ |
| [drm/amd/display: make hubp1_wait_pipe_read_start() static](https://lore.kernel.org/amd-gfx/20220415182014.278652-1-tales.aparecida@gmail.com/) | Accepted |
| [Update AMDGPU glossary and MAINTAINERS](https://lore.kernel.org/amd-gfx/20220415195027.305019-1-tales.aparecida@gmail.com/) | Accepted |
| [drm/amd/display: fix overflow on MIN_I64 definition](https://lore.kernel.org/amd-gfx/20220811204327.411709-2-tales.aparecida@gmail.com/#t) | Accepted |
| [drm/amd/display: fix minor codestyle problems](https://lore.kernel.org/amd-gfx/20220811204327.411709-3-tales.aparecida@gmail.com/) | Accepted |
| [drm/amd/display: remove unneeded defines from bios parser](https://lore.kernel.org/amd-gfx/20220821062528.13416-1-tales.aparecida@gmail.com/) | Accepted |

* Reviewed [Documentation/amdgpu/display: describe color and blend mode properties mapping](https://lore.kernel.org/amd-gfx/20220716222529.421115-1-mwen@igalia.com/)
* Reviewed [drm/amd/display: remove unreachable code](https://lore.kernel.org/amd-gfx/20220812031911.62729-1-jiapeng.chong@linux.alibaba.com/)
* Asked for a cherry-pick of two commits from [torvalds/master](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/) to [fix compilation errors on GCC12](https://lore.kernel.org/amd-gfx/20220714024748.29696-1-tales.aparecida@gmail.com/). The message didn't receive any replies, but soon after the branch got rebased, fixing the problem.

# Acknowledgment

First, I would like to thank the X.org Foundation for accepting my GSoC proposal,
for what I'll be eternally grateful.

Next, thanks to AMD for donating the RX580 GPU which powered my testing system,
and alongside Igalia allowed my mentors to take part in the GSoC program.

Moreover I would like to thank The Linux Foundation, which will enable my trip
to Dublin, to attend the Linux Plumbers Conference.

There were plenty of great interactions with the DRM community, AMD, and KUnit engineers,
for which I'm very thankful, specially David Gow which promptly reviewed most of my team's patches.

And finally, thanks for my mentors and the other mentees with whom I've worked closely this summer,
any of this would be possible without their support.

**Mentors:**
- [André "Tony" Almeida](https://andrealmeid.com/) (Igalia)
- [Melissa Wen](https://melissawen.github.io/) (Igalia)
- [Rodrigo Siqueira](https://siqueira.tech/) (AMD)

**Mentees:**
- [Isabella Basso](https://crosscat.me)
- [Maíra Canal](https://mairacanal.github.io)
- [Magali Lemes](https://magalilemes.github.io/)

# Next steps

Of course this is not farewell! It's more like reaching the end of a game's tutorial.

My first step, thanks to The Linux Foundation and my mentors, will be attending
in-person the Kernel Testing & Dependability Micro Conference at the
Linux Plumber Conference, where I hope to show what we've achieved and to discuss
about the best way to organize the tests, which is what I've invested most of
my time, so please keep an eye out for my report about it in my personal blog
in the following weeks.

In regards to coding, beyond following through with my patches under review,
I intend to write the tests for the DCE functions I proposed initially and
maybe extend to other generations, now that we realized that the physical
device is not needed in order to write unit tests. I might even look for other
modules which would benefit from unit tests, like VKMS.

Finally, I'll also try to get involved with the [KernelCI](https://github.com/kernelci/kernelci-core/) project,
where I could even employ my web development experience. In order to do that I'll be
attending virtually the monthly [Automated Testing Call](https://elinux.org/Automated_Testing#Conference_call).

---

Thanks for reading.
