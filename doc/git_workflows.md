# Git workflows

## Centralized workflow

With centralized workflow all contributors are pushing commits into same branch.

This workflow applies to small team.

![centralized workflow](data/git_workflow_centralized.png "centralized workflow")

## Feature branch workflow

Branches are created for business domain feature sets. Subteams work on different branches and maintainer integrates these feature branchs when they are ready.

![feature branch workflow](data/git_workflow_featurebranch.png "feature branch workflow")

## Gitflow workflow

Basically Gitflow is derived from Feature branch workflow with some conventions.

It defines main/develop/hotfix/feature branches for different purposes and there are some rules defined on how to work with these branches during different period of project.

[here](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) has detailed information

## Forking workflow

With forking workflow every developer has his/her own server-side repository as remote/origin. This local server-side repository is forked from another centralized server-side repository -- remote/upstream.

Developers can use any applicable workflow in their own local repository and once commit is ready a pull request can be generated for remote/upstream to merge.

[HOME](../README.md) | [PREV](advanced_topics.md) | [NEXT](code_review.md)
