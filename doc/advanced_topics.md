# Advanced Topics

## merge vs rebase

Merge and rebase are doing same thing in different ways and both of them integrate changes from a branch to another.

Let's say we have a feature branch from main as following shows:

  ![git forked](data/git_forked_commit_diagram.png "git forked commit diagram")

Merge creates new commit with changes (and commit histroy of both branches) and add to target branch. The following shows merge main to feature branch:

  ![git merge](data/git_merge_diagram.png "git merge")

Merging is non-destructive operation and existing branch are not changed in any way.

Rebase does the following steps:

- take away and save temporarily the commits after common commit
- fast forward to the new common commit
- re-apply each of saved commit and generate new one for each of them

The following diagram shows the result of a rebasing feature branch from main:

  ![git rebase](data/git_rebase_diagram.png "git rebase")

As you can see the commit history is altered.

### example

The following example [^1] shows the difference between merge and rebase:

![simple diverged branch](data/git_simple_divergent_hist.png "simple diverged branch")

![merge](data/git_integration_with_merge.png "merge")

![rebase](data/git_integration_with_rebase.png "rebase")

> Often, you’ll do this to make sure your commits apply cleanly on a remote branch — perhaps in a
project to which you’re trying to contribute but that you don’t maintain. In this case, you’d do your
work in a branch and then rebase your work onto origin/master when you were ready to submit
your patches to the main project. That way, the maintainer doesn’t have to do any integration
work — just a fast-forward or a clean apply

***Principles***

- **Do not rebase commits that exist outside your repository and that people may have based
work on** [^1]
- **never push --force after rebase to main branch**
- dont use rebase on main branch coz it's destructive
- rebase in feature branch and merge (fast-forward) in main branch is called ***git flow***
- if you are not sure use merge

## reset vs revert

***git reset***
git reset has three different modes:

- --soft

  Only moves the HEAD pointer and the staged snapshot and working directory are not altered in any way.

- It does not alter commit history.
- It does not change the staging area or index.
- It lets you quickly "undo" a commit while keeping your changes staged and worktree intact.

```bash
# usually used to squash commits, such as git rebase -i
git reset --soft HEAD~3
git commit . -m "squashed commits"
```

- --mixed
  
  The staged snapshot is updated to match the specified commit, but the working directory is  not affected. This is the default option. This is usually used to hold on commiting/push staged snapshot.

- --hard
  
  The staged snapshot and the working directory are both updated to match the specified commit.
  - Moves the current branch head to a different commit
  - Resets the index to match the commit
  - Discards any changes in the working directory
It effectively undoes all commits after the specified commit, and also undoes any changes in tracked files since that commit.
  
  ***Be very careful to do this and never do this in public repo***

***git revert***

git revert drop some commits by create new commit

## git rm/clean

- git rm update staging index as well compared to rm only
- git clean : get rid of unstaging files

## submodule

Git submodules allow you to keep a Git repository as a subdirectory of another Git repository. This is useful when you want to have separate repositories for different parts/projects, but also want to keep them organized within one main repository.
Some common use cases for Git submodules are:

- Managing libraries or dependencies
- Including third party code in your project
- Keeping reusable components separate but still bundled

To add a submodule, you use the git submodule add command:
  
```bash
git submodule add https://github.com/user/repo.git path/to/submodule
```

This will:

- Add the submodule to your repo at the given path
- Pull down the submodule repo at that commit
- Create an entry in .gitmodules with the submodule info
- Create an entry in .git/config with the submodule info

To initialize submodules in a cloned repo, run:

```bash
git submodule update --init --recursive
```

This will pull down the correct versions of all submodules recorded in the main repo.

### example

```bash
[submodule "package/mypackage/src/3rdparty/cppzmq"]
    path = package/mypackage/src/3rdparty/cppzmq
    url = https://github.com/zeromq/cppzmq.git
[submodule "package/mypackage/src/3rdparty/variant"]
    path = package/mypackage/src/3rdparty/variant
    url = https://github.com/mpark/variant.git
[submodule "package/mypackage/src/3rdparty/googletest"]
    path = package/mypackage/src/3rdparty/googletest
    url = https://github.com/google/googletest.git
```

## comment

It's very important to write a good comment of commit not only for others to understand the commit but also for all to track the history of repository in the future if needed -- and it is definitely needed.

```text
commit eb0b56b19017ab5c16c745e6da39c53126924ed6
Author: Pieter Wuille <pieter.wuille@gmail.com>
Date:   Fri Aug 1 22:57:55 2014 +0200

   Simplify serialize.h's exception handling

   Remove the 'state' and 'exceptmask' from serialize.h's stream
   implementations, as well as related methods.

   As exceptmask always included 'failbit', and setstate was always
   called with bits = failbit, all it did was immediately raise an
   exception. Get rid of those variables, and replace the setstate
   with direct exception throwing (which also removes some dead
   code).

   As a result, good() is never reached after a failure (there are
   only 2 calls, one of which is in tests), and can just be replaced
   by !eof().

   fail(), clear(n) and exceptions() are just never called. Delete
   them.
```

[here](https://cbea.ms/git-commit/) is a good post on how to write a good comment of commit.

[^1]: [git merge vs rebase](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

[HOME](../README.md) | [PREV](basic_usage.md)| [NEXT](git_workflows.md)
