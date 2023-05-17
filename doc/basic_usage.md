# Basic git usage

## git config

   ```bash
  git config --global user.name=${YOUR_USERNAME}
  git config --global user.email=${YOUR_EMAIL_ADDRESS}
  git config --global -l
  ```

## git init

  ```bash
  # example to push local repo to github.com
  
  # create new repo in github.com
  mkdir gittest
  cd gittest
  git init
  echo "test" > README.md
  git add README.md
  git commit . -m "init commit"
  git remote add origin git@github.com:arithdon/gittest.git
  git push origin master
  ```

## git clone/pull/fetch

  ```bash
  # clone:  get entire repo into local storage
  git clone git@github.com:arithdon/gittest.git
  
  # clone only one branch
  git clone -b main --single-branch git@github.com:arithdon/gittest.git
  
  # fetch: remote repo -- keep local repo intact
  git fetch upstream

  # pull: fetch and merge -- update local repo
  git pull # git push origin main
  ```

## git status

## git diff

```bash
# set external diff utils to have graphic diff
diff.external=~/bin/git-diff.sh
```

## git show

## git log

  ```bash
  # git log --grep
  git log --grep="main"
  # git log -S function_name
  git log -S rpp_log --pretty=format"%h %an %s" package/rppslave/refactor/

  # git log -- file_path
  git log --pretty="%h - %s" --author='fengbin dong' --since="2021-01-01" --before="2021-11-01" --no-merges -- qca/src/qca-wifi/
  ```

## git blame

  ```bash
  # git blame file
    
  # determine which commit and committer was responsible for code in file (most recently)
  git blame -L 54,81 package/rppslave/refactor/wlan/phycfg_apply.c
    
  # -C where sections of the code originally came from
  git blame -C -L 54,81 package/rppslave/refactor/wlan/phycfg_apply.c
  ```

## git branch

  ```bash
  git branch -a # list branches
  git checkout -b dev # create new 'test' branch
  git push --set-upstream origin test # upload local branch to remote
  git checkout main
  git branch -d test # delete test branch
  git push origin --delete test # delete remote test branch
  ```

## git stash

  ```bash
  # - shelve changes
  # - switch dev env
  git stash -u save tmpfix
  git stash list
  git stash pop stash@{0}
  ```

## git commit

```bash
# git commit - put staged objects into local repo as commited state
git commit

# git commit file - based on file's staging history commit objects into local repo
git commit .
```

## git push

## git checkout

```bash
# switch to different branch
git checkout ${BRANCH_NAME}

# drop changes in staging state
git checkout ${COMMIT_ID} ${file}
```

## git apply

```bash
# generate patches
git diff --no-ext-diff > temp.patch
# apply patch
git apply temp.patch
```

## git tag

```bash
# create a tag
git tag -a 2022-12-10
# push tag to remote repo
git push origin 2022-12-10
```

## git ls-files

```bash
git ls-files --stage
100644 216ee91d987891d8c937288b815d2480379ce6d5 0       a.txt
100644 3971ff6004bbb6d0058dd8f851e3b925dfb90e62 0       b.txt
100644 f54307eff53e460426420b7b69b821fc5cf6ebbd 0       c.txt
```

## git cat-file
  
```bash
# git cat-file -p ${COMMIT_ID}
git cat-file -p 3971ff6004bbb6d0058dd8f851e3b925dfb90e62
dev1
```

## git verify-pack

```bash
git verify-pack -v objects/pack/pack-daf1e9ac2feed87501669a9e31a92c4699a4b8a3.pack
09c643fa407acc6cb456e91b2e6d0cbce90c436a commit 213 143 12
ea77ee704760498159d235d285483bbaa9f138dd commit 213 143 155
5a18f5673fb86d7c0f0220cd732f31ddb57ab8af commit 170 115 298
f6d2bb936a4369c579cf65c7b280bfa19c79aa18 tree   66 71 413
216ee91d987891d8c937288b815d2480379ce6d5 blob   6 15 484
3971ff6004bbb6d0058dd8f851e3b925dfb90e62 blob   5 14 499
c6304a3b1a52cb5f743b068ffb8299b5c5428e92 tree   33 44 513
65a457425a679cbe9adf0d2741785d3ceabb44a7 tree   33 44 557
e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 blob   0 9 601
non delta: 9 objects
objects/pack/pack-daf1e9ac2feed87501669a9e31a92c4699a4b8a3.pack: ok
```

[HOME](../README.md) | [PREV](git_internals.md) | [NEXT](advanced_topics.md)
