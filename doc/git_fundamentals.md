# Git fundamentals

## Porcelain and Plumbing

As mentioned that git is a command line based application and commands are categorized as either "porcelain" or "plumbing" commands. This refers to the level of abstraction they operate at.

Porcelain commands are higher level, aimed at end users, and hide implementation details.

Plumbing commands are lower level, aimed at Git experts, and expose more of Git's internal workings.

Some ways to tell if a Git command is porcelain or plumbing:

- Porcelain commands usually have friendly names, e.g. git add, git commit, git push. Plumbing commands usually include "git-" and are more obscure, e.g. git-receive-pack, git-unpack-file, git-apply.
- Porcelain commands have more options and functionality aimed at convenience, plumbing commands have minimal options focused on one task.
- Porcelain commands implicitly pass through plumbing commands under the hood. So a porcelain command may execute several plumbing commands to achieve its goal.
- Plumbing commands operate on raw Git data - commits, objects, refs, etc. Porcelain commands operate on higher level concepts - files, directories, diffs.
- The output of plumbing commands is bare and machine readable. Porcelain command output is formatted for humans.
- Plumbing commands are rarely updated or changed to maintain Git's internal API. Porcelain commands often gain new options and functionality.

**TIP: the commands that are listed in "git help" output are porcelain commands (this could be wrong in some revisions of git).**

Some examples of porcelain vs plumbing commands:

| Porcelain | Plumbing |
|:--|:--|
| git add   | git-unpack-file|
| git commit   | git-commit-tree|
| git push   | git-receive-pack|
| git status   | git-update-index|
| git log   | git-rev-list|

## git database

The Git database contains all the metadata and objects for a Git repository. It is stored in the **.git** folder of the repository.

The Git database contains:

- The repository metadata (url, commits, tags, etc)
- The object store which contains:
  - Blobs: The raw file data
  - Trees: Represent folders and point to blobs or other trees
  - Commits: Point to a tree and references parent commits
- References which include:
  - Heads: Point to the latest commit on a branch
  - Remotes: Point to the latest commit on a remote tracking branch
  - Tags: Point to a specific commit

Some key files in the Git database are:

- HEAD: Points to the current branch reference
- config: Contains repository configuration
- index: The staging area/index file
- objects/: Where all object files are stored
- refs/: Contains references like heads, remotes and tags

## Integrity

Everything in Git is checksummed before it is stored and is then referred to by that checksum.

SHA-1 hash is used to generate 40-character string composed of hexadecimal characters (0–9 and a–f) that calculated based on the contents of a
file or directory structure in Git.

```bash
bf79d94f7674973921c80b3f477350d1e02814
```

> A higher probability exists that every member of your programming team will be attacked and killed by wolves in unrelated incidents on the same night than that Git will ever accidentally misidentify a single change in your project.

## Objects

There are different types of objects: blob, tree, commit, tag (annotated tag).

```bash
# list all objects
tree -a .git/objects
.git/objects
├── 1d
│   └── 0af9585bbca463698bbf2bea664ddf463c4a6a
├── 20
│   └── 4118454f2cc15b4c4ef8317d7d93ea2e783d65
├── 60
│   └── 4269e8750c600f584224a7ac8928faceba9336
├── 6b
│   └── 70f27936b34ef2593034ebd49cfd8d3d628492
├── 7b
│   └── 206957c992a23d94b535d91a4111cfeca9b1b4
├── 96
│   └── 1b3b587e5935237d7c2f62106687098b896de3
├── 98
│   └── 33ca3437610cc5f51013099b2110200e28a5b6
├── 9a
│   └── 0b5ccdd2ff446bbd1d929abdaa8f2b2db0bd3b
├── e5
│   └── 16d5e8e68a70aaa6a696bc8e7c714c88409b9f
├── f5
│   └── 4307eff53e460426420b7b69b821fc5cf6ebbd
├── info
└── pack
    ├── pack-daf1e9ac2feed87501669a9e31a92c4699a4b8a3.idx
    └── pack-daf1e9ac2feed87501669a9e31a92c4699a4b8a3.pack

12 directories, 12 files
```

The following diagram[^1] shows the relationship between the three types of objects:

![objects tree](data/git_objects_tree.png "git objects tree")

### loose vs packfiles

Git stores objects in two formats: loose and packfiles.

Loose objects are individual compressed files on disk containing objects such as:

- Blobs (file contents)
- Trees (directory structures)
- Commits (commit metadata)
- Tags (tag metadata)

These loose objects are stored in the .git/objects directory. Their filenames correspond to the hash of their content. For example:

```bash
.git/objects/56/3c64a280d146db1d02784cf5f12709f6dcd9c  # A blob object
.git/objects/4b/825dc642cb6eb9a060e54bf8d69288fbee4904  # A commit object
.git/objects/b2/5a3f2617f864e935bdaae8415ddaaa5eaf2033  # A tag object 
```

As a repository grows, the number of loose objects becomes less scalable. That's where packfiles come in.

To optimize storage and performance, Git will periodically compress multiple loose objects into "packfiles" - binary files containing many Git objects. Git can then access objects in packfiles efficiently while taking up less disk space.

Packfiles are stored in the .git/objects/pack directory. When Git encounters loose objects that could benefit from being packed, Git's git gc process compresses them into packfiles.

## Index

The index is a binary file in .git/index that contains a sorted list of path names, each with permissions and the SHA-1 of a blob object.

```bash
# list all objects
git ls-files --stage
100644 1d0af9585bbca463698bbf2bea664ddf463c4a6a 0 README.md
```

## Areas and states

There are three areas in local workspace that contain git objects in different states:

### workspace areas

![areas in workspace](data/git_repository_components.png "areas in workspace")

### workspace areas transition

- modified file, not staged , not commited , in work directory
- staged file, ready to commit , in staging area
- commited file, in local database

![area transition](data/git_three_states.png "area transition")

### file life cycle

![file life cycle](data/git_file_lifecycle.png "file life cycle")

## Branch

Branch is just pointer to a commit object.

The following diagram[^2] shows different branches in a repository:

![branches](data/git_branches.png "branches")

## Tag

A tag is a named pointer to a Git commit. It provides a way to mark specific commits in your Git repository that can be used as:

- Versioning releases of your software (v1.0.0, v2.0.0, etc)
- Indicating stable releases, beta versions, etc
- Marking deployments of your app
- Creating named checkpoints that can be revisited later

There are two main types of tags in Git:

- Lightweight tags - Simply a pointer to a specific commit. **This doesnt get its own object in the Git database.**
  
  Created with

```bash
git tag <tagname>
```

- Annotated tags - Contain additional metadata (tagger, date, message) and point to a commit. Created with

```bash
 git tag -a <tagname> -m "<message>".
```

```bash
# list all tags
git tag -l
INIT_DRAFT # one tag in this repo

# show tag object sha1
cat .git/refs/tags/INIT_DRAF
055918dbe5766054089f4ae31bdee897c174de54

# cat tag object type
git cat-file -t 055918dbe5766054089f4ae31bdee897c174de54
tag

# cat tag object content
git cat-file -p 055918dbe5766054089f4ae31bdee897c174de54
object 7340fbdaa5d24652a66ad183a0e5382b7ad7d8d7
type commit
tag INIT_DRAFT
tagger fengbin dong <fengbin.dong@spirent.com> 1684226732 +0800

tag for first draft

```

## Repos in github

  ![github repos](data/repo_diagram.drawio.png "github repos")

- origin
        - default remote repo (or forked repo)
- master/main
        - default branch of repo
- upstream
        - the repo that fork from

[^1]: <https://www.freecodecamp.org/news/content/images/2020/12/image-41.png>
[^2]: <https://www.freecodecamp.org/news/content/images/2020/12/image-48.png>

[HOME](../README.md) | [PREV](what_is_git.md) | [NEXT](git_internals.md)
