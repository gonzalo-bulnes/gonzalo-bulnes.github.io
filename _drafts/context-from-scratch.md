---
layout: post
title:  Context from Scratch
date:   2015-11-05
categories: blog gardening_with_git
tags: [recurse_center, gardening_with_git]
author: Gonzalo Bulnes Guilpain
meta: Gardening with Git
teaser: Gardening with Git series. Building the notion of context from a tree representation of Git repositories.
---

This is the first post of the _Gardening with Git_ series. The goal of the series is to build a model for Git and to provide insight about conventions and practices which allow to use Git not only for source version control, but also for knowledge sharing and communication. While the conventions and practices should make Git a wonderful ally in the operation of a software craftspeople team, we want the model to be good enough to predict the outcome of the most common git commands.

In this post, we'll first build a tree representation of Git repositories, then break it to learn from its limitations, and finally fix it to make it the first piece of our model. As an outcome, this journey will introduce the first of the most important Git concepts: context.

## Towards a tree representation of Git repositories

### Commits

A **commit** is something that holds some _data_ (let's call it **D**), and has a reference to a _parent commit_. Let's place the parent commit below the current one.
![Graphical representation of a commit.](../../../../../images/gardening_with_git/commit.png)

Let's now draw that parent commit and call its data **C**, then it's own parent (with **B** inside maybe?), and keep iterating for a while.
![Graphical representation of a commit and its parent commit.](../../../../../images/gardening_with_git/commits.png)

We dont care really about the commits' content for now, so, before going further, let's simplify the schema by removing the **D**, **C** and **B** names.
![Simplified graphical representation of a few commits.](../../../../../images/gardening_with_git/commits-simplified.png)

Two commit can have the same parent. And have children commits on their own. Let's represent such a case.
![Graphical representation of a commit with two parent commits.](../../../../../images/gardening_with_git/commits-tree.png)

Since we have a _convention_ about writing children commits above their parents, let's simplify the schema again by removing the arrows so we can draw larger **respositories** (that's just a fancy name for a collection of commits). The relation between children and their parents is now _implicit_ but the overall schema is more readable.
![Graphical representation of a few commits implicitely ordered.](../../../../../images/gardening_with_git/commits-tree-simplified.png)

With that modification the respository looks a lot like a **tree**.
![Comparison of a collection of commits and a natural tree.](../../../../../images/gardening_with_git/tree.png)

### Branches

If there is a _tree_, there must be some **branches**! There are, indeed. But before we continue, if we'll be talking about branches, we should give them names so we know what we're talking about. Let's do that by _sticking a label_ on top of each of them.
![A branched collection of commits with sticky labels on their top.](../../../../../images/gardening_with_git/stickers.png)

I said branch labels were **stuck** to the top of the branch. That's to say while new commits are added, the label always remains on the newest one.
![The branch label sticks to its top when commits are added.](../../../../../images/gardening_with_git/sticker.png)

Now that we have a **tree representation** of Git repositories, let's make sure it fits our requirements!

## Validating the metaphor and finding its limits

The whole point of creating a tree representation of our Git repositories was being able to **predict** the outcome of the Git commands we use (if we can't predict their outcome, we can't decide which commands to use). In this section we'll _validate_ the metaphor against a few examples and check to which extent it can be used.

### Predicting the `git rebase` outcome

Let's start with the `git rebase <new base> <branch>` command. (It may sound scary [<a href="#footnote-1" id="back-1">1</a>], but for our example no prior knowledge is necessary.) Here is our starting point: we had a `master` branch with 3 commits, then we created a new branch with three more commits. After the new branch was created, a couple of new commits were added to the `master` branch.
![The context for the rebase test.](../../../../../images/gardening_with_git/rebase-test-context.png)

According to our model for a branch, the third commit from the `master` branch appears to be the _base_ of the new branch.
![The base of the new branch is the commit where both branches are in contact.](../../../../../images/gardening_with_git/branch-base.png)

Re-basing a branch is about moving it to a new base. Consequently, the expected outcome of rebasing the new branch on top of `master` is the sequence of commits: **C**, **B**, **A**, **5**, **4**, **3**, **2**, **1**.
![A branch moving from its original base to a new base.](../../../../../images/gardening_with_git/rebase-test-expectation.png)

Let's perform the `git rebase` command and compare its outcome to our expectations.
<pre><code>
$ git log --oneline --deco master
d7af2c4 (master) 5
cf2c618 4
c76f446 3
b8ea8d3 2
3e57ebf 1
$ git log --oneline --deco add-user-profile
a2d5ba1 (add-user-profile) C
e3412d7 B
7535517 A
c76f446 3
b8ea8d3 2
3e57ebf 1
$ git rebase master add-user-profile
$ git log --oneline --deco add-user-profile
4f59d31 (add-user-profile) C
59e701c B
4be1d30 A
d7af2c4 (master) 5
cf2c618 4
c76f446 3
b8ea8d3 2
3e57ebf 1

</code></pre>
The command outcome does match our expectations, yay!

**Success**! The tree representation we built allowed us to predict the outcome of a `git rebase` operation. Let's now take a closer look at the `git log` command...

### Breaking the model: the `git log` outcome

Let's go back a little bit and start from the same context than we did before. Remember: we had a `master` branch with 3 commits, then we created a new branch with three more commits called `add-users-profile`. After the new branch was created, a couple of new commits were added to the `master` branch.
![The context for the log test.](../../../../../images/gardening_with_git/log-test-context.png)

The `git log` command displays all the commits that compose a given branch [<a href="#footnote-2" id="back-2">2</a>]. According to our model, the output that we can reasonably expect from a `git log` command for the `add-user-profile` branch is the sequence of commits **C**, **B**, **A**.
![A branch commits according to the tree representation.](../../../../../images/gardening_with_git/log-test-expectation.png)

Let's perform the `git log` command and compare its outcome to our expectations.
<pre><code>
$ git log --oneline --deco add-user-profile
a2d5ba1 (add-user-profile) C
e3412d7 B
7535517 A
c76f446 3
b8ea8d3 2
3e57ebf 1

</code></pre>
Oh, our expectations were not fulfilled and the sequence of commits we got was unexpected. In fact the **C**, **B**, **A** commits were displayed, but so did the **3**, **2** and **1** commits.

This time, our tree representation didn't reflect **the way Git defines branches**. While our tree model suggests that a branch starts at the branch base, `git log` displays all the commits from the top of the branch down to the initial commit.
![Comparison of the branches representations according to `git log` and our tree model.](../../../../../images/gardening_with_git/log-test-failure.png)

Let's acknowledge that fact and draw our entire repository using that branch definition.
![Comparison of a Git repository representations according to `git log` and our tree model.](../../../../../images/gardening_with_git/branches.png)

That new repository representation doesn't look much like a tree anymore, but it matches closely the `git log` **behaviour**, and would allow us to predict the command outcome accurately.

**So, what's the balance?** On the first hand, our tree representation was really useful to predict the `git rebase` outcome, but it was quite missleading when dealing with `git log`. On the other hand, the repository representation which does match the `git log` behaviour is really simple, but it doesn't seem to provide any clue about how the `git rebase` command may behave and doesn't tell much about _several commits having the same parent_.

## Interpreting the metaphor limitations

### Fixing the tree representation: domains of validity

Let's create an **alternative** tree representation that we can use it to predict the `git log` command outcome.
![A fat-tree alternative graphical representation of a git repository.](../../../../../images/gardening_with_git/alternative-tree-representation.png)

Pretty neat, isn't it? It _is_ a tree, no doubt about that, and also a good complement to tree representation which we already use to predict the `git rebase` behaviour.

Now, we have **two trees**: a _skinny_ tree and a _fat_ tree, which both model properly the Git behaviour of different kinds of commands. Understanding the difference between both kinds of commands is the key to determine **which representation we should use** to predict the output of _any_ given Git command.
![The two tree representations of a git repository used to predict the Git commands outcome.](../../../../../images/gardening_with_git/tree-representations.png)

What makes the `git rebase` and `git log` commands different? The former deals with branches in a _collective_ manner, while the latter does treat them _individually_. You only need _one_ branch to use `git log`, but you need at least two of them to perform a `git rebase` operation.

| Operations dealing with branches collectively | Operations on individual branches  |
|:----------:|:-------------:|
| `rebase`   | `log`         |
| `merge`    | `branch`      |
|            | `checkout`    |

It happens the **skinny tree representation** is a valid model for any Git command that deals with branches _collectivellly_. And the **fat tree representation** is a valid one for any Git command that operates on _individual_ branches [<a href="#footnote-3" id="back-3">3</a>].
![The domains of validity of both tree representations of a git repository used to predict the Git commands outcome.](../../../../../images/gardening_with_git/tree-representations-domains.png)

### Notion of context

So what's the big difference between considering the Git branches **individually** or **collectively**? A branch, as the `git log` command shows, is composed by **all** the commits from its very top, down to the repository's _initial commit_. It is a whole single sequence of commits. That's why, when considered individually, the _fat tree representation_ is the one that works best.

<!-- When you add a **second branch** to the scenario, however, something amazing happens. Instead of getting _two_ distinct sequences of commits, you get **three** of them: -->

When you add a **second branch** to the scenario (let's call it the _reference_ branch), something amazing happens. Instead of getting _two_ distinct sequences of commits, you get **three** of them:

<!-- (Let's call that branch the _reference_ branch by the way.) -->

1. all the commits which are unique to the **first** branch
1. all the commits which are unique to the second branch (the **reference** branch)
1. all the commits that **both branches have in common**

Often when you consider a branch among other branches you are interested in what is _unique_ about that branch. However, what is unique about a branch is not enough to define it entirely. A person is not only what makes her unique, but also everything she shares with others: her **culture**. A branch is not only what makes it unique, but also what it shares with other branches: its **context**.

And exactly as with people, you won't get the best from what makes a branch unique unless you understand in which context it is evolving.
![The definition of the context for a feature branch.](../../../../../images/gardening_with_git/feature-context.png)

Because of the importance of being able to **identify the context** when you're working with branches, let's take a few example to stress the following point: _the context depends on_ **both** _(all) the branches which are involved in the scenario_. Therefore, there is no such thing as a branch context _per se_ and the context will be different if you compare your branch to one branch or another. As we can observe, the _skinny tree representation_ is particulary fitted to reason about the context-dependent scenarios.
![Different contexts for a branch in different scenarios.](../../../../../images/gardening_with_git/example-contexts.png)

Let's hold that thought: _depending on the reference branch we choose, what we call the <em>context</em> and what we call our <em>feature</em> will change slightly_.

That idea of a **moving** context, which changes depending on our goals as for the branch opens the way to a _generalization_ of the context concept to a _single_ branch: once that we identified the commits we're focusing on (which we define as being _the&nbsp;feature_), each and every single commit down to the _initial commit_ are that feature's **context**.
![The generalization of the context idea to a single branch.](../../../../../images/gardening_with_git/single-branch-context-generalization.png)

To recap:

1. the context depends on **both** (all) the branches which are involved in the scenario (so there is no such thing as a branch context _per-se_, and the context will be different if you compare your branch to one branch or another)
1. the concept of context can be generalized to a single branch: if you're interested in a given set of commits from a branch (let's call that sequence the _feature_), all the commits from that branch down the parent relationship are the context in which your feature is defined

Identifying the current context when working with branches is something that you'll do often and which becomes easier with practice, so don't worry too much about that! Also, we'll see more examples in the next posts of this series.

## Conclusion

Let's wrap up. We've built a tree representation of a Git repository, which allowed us to determine the outcome of some Git commands. Then we streched it to find its limitations and we worked around them.

These limitations were caused by a fundamental difference between operations which involve several branches, for which we were able to define a concept of _context_, and those which involve a single branch, and therefore are free of any context.

With that in mind, the alternative tree representation we built to work around the so-called _skinny tree representation_ limitations made sense in a more general way, beyond its initial workaround status. That's why we'll talk of a _tree representation_ in a general way from now on, understanding that one of the two alternative representations does apply, depending on the Git commands we're working with.

Finally, we were able to put together a generalization of the context idea for isolated branches, which we'll find useful when talking about context adaptations.

In the next post of this series, the concept of context will allow us to understand different merging scenarios, and drive us toward our first branch management conventions. Stay tuned!

<ol id="footnotes">
  <li class="footnote" id="footnote-1">[<a href="#back-1">1</a>] Because the <code>git rebase</code> command is potentially destructive it <a href="https://www.ietf.org/rfc/rfc2119.txt" title="RFC 2119">SHOULD</a> sound scary.</li>
  <li class="footnote" id="footnote-2">[<a href="#back-2">2</a>] The <code>--oneline</code> option will ensure each commit is displayed in a single line, and is useful to save some space. The <code>--decorate</code> option will print any branch <em>sticky label</em> that could be associated with the displayed commits. We'll use both of them.</li>
   <li class="footnote" id="footnote-3">[<a href="#back-3">3</a>] That's to say you can predict the behaviour of any command that deals with branches <em>collectively</em> using the <strong>skinny tree representation</strong>. And you can predict the behaviour of any command that operates on <em>individual</em> branches using the <strong>fat tree representation</strong>.</li>
</ol>

  [should]: https://www.ietf.org/rfc/rfc2119.txt
