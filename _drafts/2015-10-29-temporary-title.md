---
layout: post
title:  Context from Scratch
date:   2015-10-29
categories: blog gardening_with_git
tags: [recurse, gardening_with_git]
author: Gonzalo Bulnes Guilpain
meta: Gardening with Git
teaser: Gardening with Git series. Building the notion of context from a tree representation of Git repositories.
---

This is the first post of the _Gardening with Git_ series. The goal of the series is to build a model for Git, good enough to predict the outcome of the most common Git commands, and to provide insight about conventions and practices which make Git a wonderful ally in the operation of a software craftpeople team by favorizing not only source code version control, but also knowledge sharing and communication.

In this post, we'll build a tree representation of Git repositories, then break it to learn from its limitations, and finally fix it to make it the first piece of our model. As an outcome, this journey will introduce the first of the most important Git concepts: context.

#### Towards a tree representation of Git repositories

##### Commits

A **commit** is something that holds some _data_ (let's call it **D**), and has a reference to a _parent commit_. (Because the parent is older, let's draw is below the current commit.)
![Graphical representation of a commit.](../../../../../images/gardening_with_git/commit.png)

We can draw that parent commit (because it's older, let call its data **C**), then it's own parent (with **B** inside maybe?), and keep iterating for a while.
![Graphical representation of a commit and its parent commit.](../../../../../images/gardening_with_git/commits.png)

We dont care really about the commits' content for now, so, before going further, let's simplify the schema by removing the **D**, **C** and **B** names.
![Simplified graphical representation of a few commits.](../../../../../images/gardening_with_git/commits-simplified.png)

Two commit can have the same parent. And have children commits on their own. Let's represent such a case.
![Graphical representation of a commit with two parent commits.](../../../../../images/gardening_with_git/commits-tree.png)

Since we have a _convention_ about writing children commits above their parents, let's simplify the schema again by removing the arrows so we can draw larger **respositories** (that's just a fancy name for a collection of commits). The relation between children and their parents is now _implicit_ but the overall schema is more readable.
![Graphical representation of a few commits implicitely ordered.](../../../../../images/gardening_with_git/commits-tree-simplified.png)

With that modification the respository looks a lot like a **tree**, doesn't it?
![Comparison of a collection of commits and a natrural tree.](../../../../../images/gardening_with_git/tree.png)

##### Branches

If there is a _tree_, there must be some **branches**! There are, indeed. But before we continue, if we'll be talking about branches, we should give them names so we know what we're talking about. Let's do that by _sticking a label_ on top of each of them.
![A branched collection of commits with sticky labels on their top.](../../../../../images/gardening_with_git/stickers.png)

I said branch labels were **sticked** to the top of the branch. That's to say while new commits are added, the label always remains on the newest one.
![The branch label sticks to its top when commits are added.](../../../../../images/gardening_with_git/sticker.png)

Now that we have a **tree representation** of Git repositories, let's make sure it fits our requirements!

#### Validating the metaphor and finding its limits

The whole point of creating a tree representation of our Git repositories was being able to **predict** the outcome of the Git commands we use (if we can't predict their outcome, we can't decide which commands to use). In this section we'll _validate_ the metaphor against a few examples and check to which extent it can be used.

##### Predicting the `git rebase` outcome

Let's start with the `git rebase <new base> <branch>` command. (It may sound scary [<a href="#footnote-1" id="back-1">1</a>], but for our example no prior knowledge is necessary.) Here is our starting point: we had a `master` branch with 3 commits, then we created a new branch with three more commits. After the new branch was created, a couple of new commits were added to the `master` branch.
![The context for the rebase test.](../../../../../images/gardening_with_git/rebase-test-context.png)

According to our model for a branch, the third commit from the `master` branch appears to be the _base_ of the new branch.
![The base of the new branch is the commit where both branches are in contact.](../../../../../images/gardening_with_git/branch-base.png)

Re-basing a branch is about moving it to a new base. Consequently, the expected outcome of rebasing the new branch on top of `master` is the sequence of commits: **C**, **B**, **A**, **5**, **4**, **3**, **2**, **1**.
![A branch moving from its original base to a new base.](../../../../../images/gardening_with_git/rebase-test-expectation.png)

Let's perform the `git rebase` command and compare its outcome to our expectations. The command outcome does match our expectations, yay!
<pre><code>
$ git log --oneline --deco master
d7af2c4 (master) 5
cf2c618 4
c76f446 3
b8ea8d3 2
3e57ebf 1
$ git log --oneline --deco add-user-profile
a2d5ba1 (add-some-colors) C
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

**Success**! The tree representation we built allowed us to predict the outcome of a `git rebase` operation. Let's now take a closer look at the `git log` command...

##### Breaking the model: the `git log` outcome

Let's go back a little bit and start from the same context than we did before. Remember: we had a `master` branch with 3 commits, then we created a new branch with three more commits called `add-users-profile`. After the new branch was created, a couple of new commits were added to the `master` branch.
![The context for the log test.](../../../../../images/gardening_with_git/log-test-context.png)

The `git log` command displays all the commits that compose a given branch [<a href="#footnote-2" id="back-2">2</a>]. According to our model, the output can reasonnably expect from a `git log` command for the `add-user-profile` branch is the sequence of commits **C**, **B**, **A**.
![A branch commits according to the tree representation.](../../../../../images/gardening_with_git/log-test-expectation.png)

Let's perform the `git log` command and compare its outcome to our expectations. Oh, this time our expectations are not fulfilled and the sequence of commits we got was unexpected. In fact the **C**, **B**, **A** commits were displayed, but so did the **3**, **2** and **1** commits.
<pre><code>
$ git log --oneline --deco add-user-profile
a2d5ba1 (add-some-colors) C
e3412d7 B
7535517 A
c76f446 3
b8ea8d3 2
3e57ebf 1

</code></pre>

[This time, Git doesn't seems to define branches as our tree model does.]
![Comparison of the branches representations according to `git log` and our tree model.](../../../../../images/gardening_with_git/log-test-failure.png)

[What does that mean for our repository?]
![Comparison of a Git repository representations according to `git log` and our tree model.](../../../../../images/gardening_with_git/branches.png)

[Let's modify a little bit our tree representation so we can use it to predict the `git log` command outcome!]
![A fat-tree alternative graphical representation of a git repository.](../../../../../images/gardening_with_git/alternative-tree-representation.png)

#### Interpreting the metaphor limitations

##### Fixing the tree representation: domains of validity

##### Notion of context

#### Conclusion

<ol id="footnotes">
  <li class="footnote" id="footnote-1">[<a href="#back-1">1</a>] Because the `git rebase` command is potentially destructive it <a href="https://www.ietf.org/rfc/rfc2119.txt" title="RFC 2119">SHOULD</a> sound scary.</li>
  <li class="footnote" id="footnote-2">[<a href="#back-2">2</a>] The <code>--oneline</code> option will ensure each commit is displayed in a single line, and is useful to save some space. The <code>--decorate</code> option will print any branch <em>sticky label</em> that could be associated with the displayed commits. We'll use both of them.</li>
</ol>

  [should]: https://www.ietf.org/rfc/rfc2119.txt
