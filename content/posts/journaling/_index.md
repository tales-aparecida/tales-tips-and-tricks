+++
title = "Journaling - Tales Tail"
summary = "Tales Tail is a personal journal where I try to talk about my latest work in progress."
date = 2022-09-25T12:00:00-03:00
aliases = ["tales-tail", "journal"]
+++

![A cosmetic banner stylized as a terminal that reads: journalctl -f | tee tales-tail.log](/tales-tips-and-tricks/images/tales-tail-banner.png)

"Tales Tail" is what I call my personal journal where I try to talk about my latest work in progress.

The naming is a combination of two homophones: my own name and the Unix program `tail`,
which shows the tail end of a text file or piped data.

The content is written while I'm walking through the tasks, so bear with me,
some tasks will eventually result in a proper blog post!

<!--
  The banner was based on the stylesheet provided
  by Chris Coyier at https://css-tricks.com/old-timey-terminal-styling/

<scss>
  body {
    background-color: black;
    background-image: radial-gradient(rgba(0, 150, 0, 0.75), black 120%);
    height: 100vh;
    margin: 0;
    overflow: hidden;
    padding: 2rem;
    color: white;
    font: 1.3rem Inconsolata, monospace;
    text-shadow: 0 0 5px #C8C8C8;

    &::after {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: repeating-linear-gradient(0deg,
          rgba(black, 0.15),
          rgba(black, 0.15) 1px,
          transparent 1px,
          transparent 2px);
      pointer-events: none;
    }
  }

  ::selection {
    background: #0080FF;
    text-shadow: none;
  }

  pre {
    margin: 0;
  }
</scss>

<html>
  <pre>
    <output>
$ journalctl -f | tee tales-tail.log
-- Logs begin at Thu 2022-09-12 11:57:47 -03. --
set 12 19:00:34 NM-NB32 update-notifier-crash[5672]: /usr/bin/whoopsie
set 12 19:00:52 NM-NB32 rtkit-daemon[1774]: Supervising 8 threads of 2 processes of 1 users.
set 12 19:00:52 NM-NB32 rtkit-daemon[1774]: Supervising 8 threads of 2 processes of 1 users.
set 12 19:00:52█
    </output>
  </pre>
</html>
-->