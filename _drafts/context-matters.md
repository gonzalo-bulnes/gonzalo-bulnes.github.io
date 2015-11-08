---
layout: post
title:  Context Matters
date:   2015-11-11
categories: blog gardening_with_git
tags: [recurse_center, gardening_with_git]
author: Gonzalo Bulnes Guilpain
meta: Gardening with Git
teaser: Gardening with Git series. Introduces a mindset shift to understand which role plays the context in branch merging, and defines the corresponding branch management conventions.
---

This is the second post of the _Gardening with Git_ series. The goal of the series is to build a model for Git and to provide insight about conventions and practices which allow to use Git not only for source version control, but also for knowledge sharing and communication. While the conventions and practices should make Git a wonderful ally in the operation of a software craftspeople team, we want the model to be good enough to predict the outcome of the most common git commands.

The [previous post][previous] introduced the notion of **context** from a tree representation of Git repositories. In this post, we'll first adopt an unusual perspective to charaterize diff files so we can use them to understand what merging branches means. We'll then leverage the notion of context to understand the different possible merging scenarios, and finally build our first branch management conventions based on our observations.

## Shifting mindset: diff files as change vectors

### Diff files are executables files

Let's start this post looking at the very base of Git: the `diff` tool.



### Commits are actions packaged for re-use

### Merging branches is re-using commits

## The context role in distinct `git merge` scenarios

## Towards readable changesets: a convention

## Conclusion

  [previous]: context-from-scratch.html
