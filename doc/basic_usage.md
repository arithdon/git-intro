# Basic git usage

## setup access to github

There are two ways to access github.com: ssh and https. The ssh way is more convenient, but you need to setup ssh key first.

- ssh way: use ssh key to access github.com

```bash
# generate ssh key pair
ssh-keygen -t ed25519 -b 4096 # generate ed25519 key pair
cat ~/.ssh/id_ed25519.pub # copy public key to github.com

# update ~/.ssh/config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
```

>RSA keys (ssh-rsa) with a valid_after before November 2, 2021 may continue to use any signature algorithm. RSA keys generated after that date must use a SHA-2 signature algorithm. Some older clients may need to be upgraded in order to use SHA-2 signatures.

[here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) is how to add your pub key to github.

```bash
# clone repo
git clone git@github.com:arithdon/git-intro.git
```

- https way: use https token to access github.com

see [here](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) on how to generate https token.

```bash
# generate https token in github.com
GITHUB_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# update git config
git config --global url.https://${GIT_TOKEN}@github.com/arithdon.insteadOf https://github.com/arithdon

# clone repo
git clone https://github.com/arithdon/git-intro.git
```

### **https vs ssh**

Git over HTTPS can provide faster access than Git over SSH in some cases. There are a few reasons for this:

- HTTPS uses cached credentials - After the initial login, HTTPS relies on cached credentials and does not prompt for your password again. SSH requires entering your password or SSH key passphrase each time it connects to the server.
- HTTPS has fewer restrictions - HTTPS endpoints typically have higher limits for things like Git packfile sizes, shallow clones, etc. SSH is more restrictive by nature.
- Direct file access - The Smart HTTP protocol used by Git allows direct access to .git/objects/pack files. This means Git can avoid recompressing packfiles that already exist on the server. Over SSH, Git cannot access these files directly.
- Support for transfer compression - The Git HTTP protocol supports gzip compression of data, which can make a big difference for the initial clone of a large repo. Git SSH has no compression.
- Caching proxies - Git HTTP can make use of caching proxies, since it is a standard HTTP API. This can speed up connections, especially over a slow network. SSH has no proxy support.

However, there are also some downsides to Git over HTTPS:

- Typically requires manual login - While credentials are cached, you still need to enter your username and password manually the first time. SSH keys can provide fully automated authentication.
- No SSH features - You lose features from the SSH protocol like port-forwarding, tunnelling, etc. Raw HTTPS provides only Git connectivity.
- Self-signed SSL certificates - To use HTTPS with a self-signed SSL cert, you need to disable SSL verification. This reduces the security, requiring blind trust in the endpoint.
- Less widely supported - While HTTPS support has improved, some older Git servers still lack support for Git via HTTPS. SSH is considered a "standard" way to connect and is more widely supported.

After clone a repo with ssh you can change it to https by:

```bash
git config --replace-all remote.origin.url "https://github.com/username/repo.git"

# continue to use git pull/push but with https now.
```

Note that the first time you use https you will be asked for your username and password. After that, it will be cached in your system's credential store and you won't be asked for it again.

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
  git remote add origin git@github.com:arithdon/git-intro.git
  git push origin master
  ```

## git clone/pull/fetch

- git fetch
  - Downloads data from a remote repository that you don't have yet. It retrieves data that you don't have, but doesn't merge any changes into your branch. This allows you to inspect changes before merging them in.
- git pull
  - Incorporates changes from a remote repository into the current branch. Pull performs a fetch and then a merge. This is convenient and updates your local version, but can lead to conflicts if not done carefully.
- git clone
  - Creates a local copy of a remote repository. It clones the entire source repository, creates a local branch to track the source's branch (usually master), and sets up remote tracking so you can pull/push to the remote

  ```bash
  # clone:  get entire repo into local storage
  git clone git@github.com:arithdon/git-intro.git
  
  # clone only one branch
  git clone -b main --single-branch git@github.com:arithdon/git-intro.git
  
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
  git stash save -u tmpfix
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
