---
layout: post
title:  Context from Scratch
date:   2015-10-29
categories: blog gardening_with_git
tags: [recurse, gardening_with_git]
author: Gonzalo Bulnes Guilpain
meta: Gardening with Git
---

This is the first post of the _Gardening with Git_ series. The goal of the series is to build a model for Git, good enough to predict the outcome of all Git commands, and to provide insight about conventions and practices which make Git a wonderful ally in the operation of a software craftpeople team by favorizing not only source code version control, but also knowledge sharing and communication.

In this post, we'll build a tree representation of Git repositories, then break in to learn from its limits, and finally fix it to make it the first piece of our model. As an outcome, this journey will introduce the first of the most important Git concepts: context.

##### Towards a tree representation of Git repositories

A **commit** is something that holds some _data_ (let's call it **D**), and has a reference to a _parent commit_. (Because the parent is older, let's draw is below the commit.)
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

If there is a _tree_, there must be some **branches**! There are, indeed. But before we continue, if we'll be talking about branches, we should give them names so we know what we're talking about. Let's do that by _sticking a label_ on top of each of them.
![A branched collection of commits with sticky labels on their top.](../../../../../images/gardening_with_git/stickers.png)

Branch labels are **sticky**. That's to say while new commits are added, the label always remains on the newest one.
![The branch label sticks to its top when commits are added.](../../../../../images/gardening_with_git/sticker.png)

Now that we have a **tree representation** for Git repositories, let's find its limits! (Mission 1: complete.  &#10003;)


