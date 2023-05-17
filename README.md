# Git Introduction

## Overview

A good understanding of Git is valuable for developers working collaboratively on same projects.

Git is a version control system that allows multiple developers to contribute to a repository simultaneously. It is a command-line application with a set of commands to manipulate commits and branches (explained below). This document aims to provide a TOI to help beginners get started using Git.

***This document is highly inspired by [Pro Git](https://git-scm.com/book/en/v2) and [this](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud).***

<!-- TOC -->
- [What is Git](doc/what_is_git.md)
  - [Backgroud](doc/what_is_git.md#background)
  - [Snapshots not differences](doc/what_is_git.md#snapshots-not-differences)  
- [Git Fundamentals](doc/git_fundamentals.md)
  - [Porcelain and Plumbing](doc/git_fundamentals.md#porcelain-and-plumbing)
  - [Integrity](doc/git_fundamentals.md#integrity)
  - [Objects](doc/git_fundamentals.md#objects)
  - [Index](doc/git_fundamentals.md#index)
  - [Areas and states](doc/git_fundamentals.md#areas-and-states)
  - [Branch](doc/git_fundamentals.md#branch)  
  - [Tag](doc/git_fundamentals.md#tag)
  - [Repos in github](doc/git_fundamentals.md#repos-in-github)
- [Create git repo from scratch](doc/git_internals.md)
  - [create repo with porcelain commands](doc/git_internals.md#create-repo-with-porcelin-commands)
  - [create repo with plumbing commands](doc/git_internals.md#create-repo-with-plumbing-commands)
- [Basic Usages](doc/basic_usage.md)
  - [setup access to github](doc/basic_usage.md#setup-github-access)
- [Advanced Topics](doc/advanced_topics.md)
  - [merge vs rebase](doc/advanced_topics.md#merge-vs-rebase)
  - [reset vs revert](doc/advanced_topics.md#reset-vs-revert)
  - [reflog](doc/advanced_topics.md#git-reflog)
  - [detached HEAD state](doc/advanced_topics.md#detached-head-state)
  - [submodule](doc/advanced_topics.md#submodule)
  - [comment](doc/advanced_topics.md#comment)  
- [Workflows](doc/git_workflows.md)
  - [Centralized workflow](doc/git_workflows.md#centralized-workflow)  
  - [Feature branch workflow](doc/git_workflows.md#feature-branch-workflow)
  - [Gitflow workflow](doc/git_workflows.md#gitflow-workflow)  
  - [Forking workflow](doc/git_workflows.md#forking-workflow)
  - [Create PR in github](doc/git_workflows.md#create-pr-in-github)
- [Code Review](doc/code_review.md)
  - [PR](doc/code_review.md#pr)
  - [codecollabrator](doc/code_review.md#codecollabrator)  
<!-- /TOC -->
