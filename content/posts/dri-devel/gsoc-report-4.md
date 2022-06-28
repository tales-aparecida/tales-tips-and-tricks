+++
title = "Reviewing patches"
summary = "Let's learn together how to apply patches, test them and be part of the community."
date = 2022-06-27T12:00:00-03:00
draft = false
authors = ["Tales L. Aparecida"]
categories = ["DRI-Devel"]
tags = ['gsoc', 'kernel', 'git']
featuredImage = "/tales-tips-and-tricks/images/applying-patch-to-belly.jpg"
+++

Being part of the community, is more than just writing code and sending patches, it is also keeping track of the IRC discussions and reading the mailing lists to **review** and **test** patches sent from others whenever you can.

Both environments are not the most welcoming, but there are plenty of tools from the _community_ to help parsing them. In this post I'll talk about [b4](https://github.com/mricon/b4), suggested by my GSOC mentor [André](http://andrealmeid.com/), a tool to help with applying patches.

<!-- more -->

# Applying patches

I assume you already know that when we refer to "git commits", we are basically talking about snapshots of the files in the repository ([more about that](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)); it's almost like, for each set of changes, we archived and compressed the whole repository folder an gave the result a name.

{{< notice example >}}
- v1-created-wireframes.tar.gz
- v2-minimum-testable-product.tar.gz
- v2.1-fixed-download-icon.tar.gz
- v3...
{{< /notice >}}

When working in a large project with so many people, like we have in the Linux Kernel community, it would be impractical to send a file containing the whole repository just to show some changes in some files, specially in the old days, when there probably wasn't even that much bandwidth. So, in order to _share your workings_ with the community you just have to tell them "add X to line N, remove Y from the following line", in other words, you have to share only the **differences** you brought to the code.

There is a command to convert your commits into these messages showing only the "diffs" in your code: [`git format-patch`](https://git-scm.com/docs/git-format-patch). It's worth mentioning that Git uses its own enhanced format of `diff` (see [`git diff`](https://git-scm.com/docs/git-diff)), which tries to humanize and contextualize some changes, either by recognizing scopes in some languages or simply including surrounding lines in the output. So, lets say you created _a couple_ commits based on [`master`](https://github.com/torvalds/linux) and want to extract them as _patches_, you could run `git format-patch master`, which would create _a couple_ numbered files. You could then send them via email with [`git send-mail`](https://git-scm.com/docs/git-send-email), but that's another talk, my point here was just to introduce the concept of patches, you can read more at https://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project.


{{< notice note >}}
Nowadays there are plenty of source-code hosts, like Github and Gitlab, that provide an alternative to email patching through [Pull](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)/[Merge Request](https://docs.gitlab.com/ee/user/project/merge_requests/).
{{< /notice >}}

Now, lets say somebody has already sent their patch to some mailing list, like https://lore.kernel.org/all/20220627161132.33256-1-jose.exposito89@gmail.com/. How can you assert that their code compiles and works as described? 


You could find the link and download the `mbox.gz` file from the [lore.kernel.org](https://lore.kernel.org/all/20220627161132.33256-1-jose.exposito89@gmail.com/) page, or find the series at [patchwork.kernel.org](https://patchwork.kernel.org/project/dri-devel/list/?series=654239) to do the same, which then would allow you to use [`git am`](https://git-scm.com/docs/git-am) to apply the patches, recreating the commits in your local environment. That process is easy enough but it can be improved as far as running a command over the `lore.kernel.org` URL with [`b4`](https://github.com/mricon/b4).

{{< notice info >}}
B4 is a helper utility to work with patches made available via a public-inbox archive like lore.kernel.org. It is written to make it easier to participate in a patch-based workflows, like those used in the Linux kernel development.
{{< /notice >}}


# B4 - it's not an acronym, it's just a name

**B4** is a Python package and can be easily installed with `python3 -m pip install --user b4`. I'd suggest using a [virtual environment](https://docs.python.org/pt-br/3/library/venv.html) to avoid problems with dependencies, but this post won't cover that.

It comes with a helpful `b4 --help`, which tells us that, to apply the mentioned patch series you'd just need to run:

```
b4 am https://lore.kernel.org/all/20220627161132.33256-1-jose.exposito89@gmail.com/
```

Which will download the patch series as a mbox file and the cover letter as another, so that you could then use `git am` on it the former. With some luck (and communication), everything will apply without any conflicts.

That's it, good luck on your reviews and thanks for reading!

---

"applying patch to belly" by The EnergySmart Academy is licensed under CC BY-NC-SA 2.0. To view a copy of this license, visit https://creativecommons.org/licenses/by-nc-sa/2.0/?ref=openverse. 