+++
title = "#TalesTail 2022-09-25 | KUnit in production?"
summary = "Personal journal entry of a work in progress: Trying to understand why people want KUnit in production Kernel"
date = 2022-09-25T18:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["Journal"]
tags = ['tales-tail', 'kernel', 'kunit', 'testing']
featuredImage = "/tales-tips-and-tricks/images/tales-tail-banner.png"
+++

One of the things we've talked at the Linux Plumbers Conference was how there are
people interested in compiling the Kernel with KUnit enabled. This discussion is
solidified by Joe Fradley's Aug 17th patch:

> [[PATCH 0/2] kunit: add boot time parameter to enable KUnit](https://groups.google.com/g/kunit-dev/c/DUC8-7EooHo)
>
> There are some use cases where the kernel binary is desired to be the same
for both production and testing. This poses a problem for users of KUnit
as built-in tests will automatically run at startup and test modules
can still be loaded leaving the kernel in an unsafe state. There is a
"test" taint flag that gets set if a test runs but nothing to prevent
the execution.
>
> This patch adds the kunit.enable module parameter that will need to be
set to true in addition to KUNIT being enabled for KUnit tests to run.
The default value is true giving backwards compatibility. However, for
the production+testing use case the new config option
KUNIT_ENABLE_DEFAULT_DISABLED can be enabled to default kunit.enable to
false requiring the tester to opt-in by passing kunit.enable=1 to
the kernel.

Knowing that there are people using KUnit in their workflow sparks joy,
but I believe these new use cases should be treated with care.
There might be some test suites that were not written taking production into consideration,
that is, they might have been written with the assumption that tests wouldn't be compiled out of a controlled environment.

Guess I'll have a look around in those test files. 🔎

There's at least one thing that we should watch out for, that is
[testing static functions](https://docs.kernel.org/dev-tools/kunit/usage.html#testing-static-functions),
which injects the test cases into the same compilation unit when the tests are compiled.
Apparently there will be some activity in that front, but for now we wait...
and by that I'm referring to that series
[adding unit tests into the AMDGPU driver](https://lore.kernel.org/amd-gfx/20220912155919.39877-1-mairacanal@riseup.net/),
which will be delayed until we understand better this Kunit-in-production use cases.

---

"Tales Tail" is a personal journal where I try to talk about my latest work in progress.
Today I was trying to understand why people want KUnit in production Kernel.
