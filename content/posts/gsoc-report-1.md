+++
title = "My experience as a Google Summer of Code Contributor - Introduction"
description = "I've been accepted as a GSOC contributor for X.Org! Let me tell you a bit about my project and the initial experience"
date = 2022-06-06T12:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["Report"]
tags = ['gsoc', 'kernel', 'amdgpu']
featuredImage = "/tales-tips-and-tricks/images/party-penguin.jpg"
+++

So it begins!

With some pushes and pulls from friends, I've been studying the Linux Graphical stack for some time now. After some minor patches to both [Mesa](https://gitlab.freedesktop.org/mesa/mesa) and the [Linux Kernel](https://lore.kernel.org/all/?q=tales.aparecida@gmail.com), I followed the [instructions](https://summerofcode.withgoogle.com/programs/2022/organizations/xorg-foundation) thoroughly and landed a successful [Google Summer of Code proposal](https://summerofcode.withgoogle.com/proposals/details/TKAqZe03):

# Introduce Unit Tests to the AMDGPU “DCE” Component

My project’s primary goal is to create unit tests using KUnit for the AMDGPU driver focused on the Display and Compositing Engine (DCE) 11.2, which will be tested on the GPU “RX 580”.

The motivation for that comes not only to assert that the APIs work as expected, but also to keep their behavior stable across minor changes in their code, which can allow for great improvement to the code readability and maintainability.

For the implementation of the tests, _we_ decided to go with the Kernel Unit Testing Framework (KUnit). KUnit makes it possible to run test suites on kernel boot or load the tests as a module. It reports all test case results through a TAP (Test Anything Protocol) in the kernel log.

There is a great probability that KUnit will have some limitations in regards to testing GPU’s drivers’ functions, so the secondary goal will be to enhance its capabilities. There will be other people working with KUnit on DCN in parallel, so there will be a lot of code review to be done as well. I will keep track of my weekly progress on my blog, reporting the challenges I will face and trying to create an introductory material that could help future newcomers.


# Mentors and Teammates

During this _summer_ I'll have by my side [Isabella Basso](https://crosscat.me) and [Maíra Canal](https://mairacanal.github.io), sharing an overall similar GSOC proposal but working with DCN, which is used by newer GPUs, and [Magali Lemes](https://magalilemes.github.io/), working on her related capstone project. We all will be mentored by three awesome FLOSS contributors:

- [André "Tony" Almeida](https://andrealmeid.com/)
- [Melissa Wen](https://melissawen.github.io/)
- [Rodrigo Siqueira](https://siqueira.tech/)


# Community

When talking about FLOSS, communication must be plenty _and time-travel compatible_ 🙃!

Jokes aside, the two main channels to chat and exchange _patches_ are IRC and mailing lists, respectively.

## Mailing Lists

Right now, I'm still overwhelmed by the volume of emails arriving (even after setting some filters). Searching for relevant threads at [lore.kernel.org](https://lore.kernel.org) has proven useful. Right now, I'm subscribed to:

- [DRI-Devel](https://lists.freedesktop.org/mailman/listinfo/dri-devel)
- [AMD-GFX](https://lists.freedesktop.org/mailman/listinfo/amd-gfx)
- [Kunit-dev](https://groups.google.com/g/kunit-dev)

## IRC channels 

IRC is an important tool used by the community to keep in touch _real time_. Similar to old-school chat rooms, there's no chat history by default, so, to circumvent that, I've been using [thelounge](https://thelounge.chat/) kindly deployed by André, which acts not only as an _IRC web client_ but also a _IRC bouncer_, meaning that it keeps me connected and stores any messages while I'm away.

I've joined the following IRC channels:

- `#kunit-usp`: Where daily discussions from our team are being held, in portuguese.
- `#kunit`: [Kunit](https://kunit.dev/) development channel.
- `#kw-devel`: [Kworkflow](kworkflow.org/) development channel.
- `#dri-devel`: Pretty active channel shared by Mesa and Kernel graphics (filled with light hearted people, highly recommend!).
- `#freedesktop`: [freedesktop.org](https://www.freedesktop.org) infrastructure and online services.
- `#radeon`: Support and development for open-source radeon/amdgpu drivers.
- `#xorg-devel`: [X.Org](https://www.x.org/) development discussion.

---

"Happy Birthday Penguin Cake" by foamcow is licensed under CC BY-NC-SA 2.0. To view a copy of this license, visit https://creativecommons.org/licenses/by-nc-sa/2.0/?ref=openverse. 