# Usage

## Housekeeping

Keeping your repositories close to the root of your storage drive helps prevent issues with maximum path lengths, especially on Windows systems, where paths longer than 260 characters can cause problems.

> [!Tip]
> I highly recommend storing your repos in a consistent place.
>
> I typically use `C:/git` or `~/git`

---

## New Repo

### Setting Default Branches

When initializing a new repository, you may want to set up the default branches according to your workflow. Here’s a script to create a new repository with `trunk` as the main branch and `develop` as a buffer branch.

```bash
#!/bin/bash

# Script to initialize a new Git repository with default branches and epoch tagging

REPO_NAME=$1

if [ -z "$REPO_NAME" ]; then
  echo "Usage: $0 <repo-name>"
  exit 1
fi

mkdir $REPO_NAME
cd $REPO_NAME
git init -b trunk
git checkout -b develop
echo "# $REPO_NAME" > README.md
git add README.md
git commit -m "Initial commit on develop branch"
git tag -a v0.0.1 -m "Epoch tag"
git checkout trunk
echo "Repository $REPO_NAME initialized with trunk and develop branches and epoch tag v0.0.1"

```

---

## Different URL types

When cloning a repository, you can use different URL types. Here’s a brief overview.

### HTTP

HTTP URLs are useful when you need to work through a firewall or proxy. However, they require you to enter your credentials each time you push changes unless you use a credential manager.

>e.g. `git clone https://github.com/username/repo.git`

### SSH

SSH URLs are preferred for their security and convenience. They use SSH keys for authentication, eliminating the need to enter your credentials for each operation.

Click [here for help](/ssh?id=ssh-setup) on setting up SSH

>e.g. `git clone git@github.com:username/repo.git`

### Git

Git URLs are similar to SSH URLs but are less common. They can only be used if both the client and server are on the same network or the server is accessible via a direct connection.

You probably won't use these, but they're listed here for completeness...

>e.g. `git clone git://github.com/username/repo.git`

---

## Commands

### Day-to-day

Here are some common Git commands you will use on a daily basis.

Clone a repository.

```bash
git clone <SSH_URL>
```

Add all changes to the staging area.

```bash
git add .
```
Commit changes with a message.

```bash
git commit -m "Commit message"
```

Pull the latest changes from the remote repository.

```bash
git pull
```

Push your changes to the remote repository.

```bash
git push
```

### Staging

Stage specific files.

```bash
git add <file1> <file2>
```

Stage all changes.

```bash
git add .
```

### Branching

Branches allow you to work on multiple features or fixes simultaneously.

Create and switch to a new local branch

```bash
git checkout -b <branch-name>
```

> [!Note]
> The above is shorthand for
> `git branch <branch-name> && git checkout <branch-name>`


Sync your local branch to the remote repository

```bash
git push -u origin <branch-name>
```

> [!Note]
> The above is shorthand for
> `git push --set-upstream origin <branch-name>`

---

### PRs (Pull Requests)

Pull Requests are used to review and merge changes from one branch to another, typically from a feature branch to a main branch.

1. Push your branch to the remote repository.
1. Create a Pull Request on the hosting platform (e.g., GitHub, GitLab).
1. Review the changes and merge the PR after approval.

---

### Submodules

Submodules allow you to include other Git repositories within a repository.

Add a submodule
```bash
git submodule add <repo-url> <path>
```

Initialise and update submodules
```bash
git submodule update --init
```

---

### Tags

---

### Versioning

Tags can be used for versioning, and git describe can show the most recent tag reachable from a commit.

```bash
git describe --tags --long --dirty
```