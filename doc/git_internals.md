# Create repo from scratch

In order to better understand git, we will create a repo from scratch with pluming commands.

## Create a repo

```bash
mkdir myrepo
cd myrepo
mkdir -p .git/objects
mkdir -p .git/refs/heads
echo "ref: refs/heads/main" > .git/HEAD

echo "m1" | git hash-object --stdin -w

# new object has hash 63a911f26fe84ea7fd8a863a636cfac908895ec9
# update index
git update-index --add --cacheinfo 100644 63a911f26fe84ea7fd8a863a636cfac908895ec9 README.md

# create file in workspace
git cat-file -p 63a911f26fe84ea7fd8a863a636cfac908895ec9 > README.md

# create commit
# before commit, we need to create a tree object
git write-tree # new tree object is 110264aaff3780009396782b62f91c400a46c8f1
# create commit object
git commit-tree 110264aaff3780009396782b62f91c400a46c8f1 -m "init commit" # new commit object e65c70a66eeec65b17eba4697b1b4afdea7041e2

# update ref
echo "e65c70a66eeec65b17eba4697b1b4afdea7041e2" > .git/refs/heads/main
```

[HOME](../README.md) | [PREV](git_fundamentals.md) | [NEXT](basic_usage.md)
