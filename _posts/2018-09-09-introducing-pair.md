---
layout: post
title:  Introducing Pair
date:   2018-09-09
categories: blog open-source
tags: [open-source]
author: Gonzalo Bulnes Guilpain
meta: Open-Source
teaser: Attribute your pairs seamlessly when committing code together, with a simple Pair command.
---

You love attributing your pairs when [committing code together][together]? I certainly do. Yet after pairing for most of the past two years, I find myself once and again forgetting to do it and finding that writing my pairs' names and email addresses is... repetitive.

Let me introduce **`pair`**, a tiny command-line tool that allows everyone to start their pairing sessions and switch pairs with a simple 🍐 command, while keeping control over what goes into their commit messages!

  [together]: https://help.github.com/articles/creating-a-commit-with-multiple-authors/

## Where can I try it?

### Releases

Binaries for official releases [<a href="#footnote-1" id="back-1">1</a>] may be downloaded from the [releases page on GitHub][releases]. This is likely the quickest and easiest way to get started.

  [releases]: https://github.com/gonzalo-bulnes/pair/releases

### Source code

Pair is written in [Go](https://golang.org). Its [source code is available on Github](https://github.com/gonzalo-bulnes/pair) along with instructions to download it and install the command using Go's tooling if that's your thing.

## Overview

<pre><code>
$ pair with "Gonzalo <gonzalo.bulnes@redbubble.com>" # that's all!

# Wanna pair swap?
$ pair with "Alice <alice@example.com>"

# Never stop pairing!
# Well, if you do:
$ pair stop

</code></pre>

### Where does the idea come from?

Code written by a pair is often better (no matter what you understand by better) than code from a single author. Partly because of that (and partly because it's fun!) people in my team started adding a note to mention who their pairs were in their commit messages.

<pre><code>
Author: Alice <alice@example.com>
Date:   Mon Sep 10 15:02:32 2018 +1000

    Fix unicorns countdown in sky rocket

    The counter was being mistakenly reset as a side-effect
    of the re-fuelling. Removed unnecessary dependency between
    the two processes.

    Fixes #283

    🍐 Bob

</code></pre>

Then, some time ago, Github introduced a convention for [creating commits with multiple authors][inspiration]. The idea was extremely similar and our annotations could now be understood by Github!

<pre><code>
Author: Bob <bob@example.com>
Date:   Tue Sep 11 14:01:44 2018 +1000

    Add rainbow slider to emergency kit

    Co-Authored-By: Alice <alice@exmple.com>

</code></pre>

I like this feature: not only it allows Github to make visible when people are pairing together, but it also stays readable in a standard Git log.

We lost the emoji, which was a bit sad, but more annoyingly the annotation became a lot longer to type, and the email part made it error-prone. Mistakes made attribution sometimes frustrating and my eagerness for copy-pasting transformed my commit process into a somewhat complex endeavour.

### How does Pair work?

Git has long supported [commit templates][template], that is, files that hold the text that populates the text editor every time you `git commit`.

What if we could put the co-author declaration in there once and for all? Well... until we swap pairs? We can! Pair does [exactly that][doc].

There are subtleties, of course, like having a global template and different local ones in some repositories, but mostly, Pair handles that for you. It makes convenient to edit the template in use with a single command, so that you can focus on what you're doing! 🍐

  [inspiration]: https://help.github.com/articles/creating-a-commit-with-multiple-authors/
  [template]: https://git-scm.com/docs/git-commit#git-commit---templateltfilegt
  [doc]: https://github.com/gonzalo-bulnes/pair/blob/master/doc/README.md

## Contributing

At the time of writing, Pair's [pre-release (v1.0.0-alpha) is available][releases]. It is stable to use, and I expect mostly to catch cross-platform or usability issues. I am interested in your experience with it, as much as with errors that I may have missed.

If you download it and use it, and can [feedback][issues] what you think and any bugs, inconsistencies that you experience, or indeed code smells that you find, I'll be happy to pair together to fix them or move forward. : )

  [issues]: https://github.com/gonzalo-bulnes/pair/issues

## Conclusion

Go [download it][releases]! (I'm kidding, no pressure.)

Pair _could, I hope_, make your pair programming sessions more enjoyable by removing one very little annoying moment from your practice. Hopefully, it can also make more visible the fact that collaboration is a two-way exchange, and everybody more keen on pairing together again!


<ol id="footnotes">
  <li class="footnote" id="footnote-1">[<a href="#back-1">1</a>] I said <em>everyone</em> in the introduction: I would indeed appreciate input from people looking for using Pair on Windows. I have built Linux and OSX binaries so far, and would love to learn what's needed to support any other platform you care about.</li>
</ol>

  [should]: https://www.ietf.org/rfc/rfc2119.txt
