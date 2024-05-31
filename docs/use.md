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

When cloning an empty repository, you may want to set up the default branches.  
Here’s a script to create a new repository with `trunk` and `develop` branches.

> Save the script as `NewRepo.sh` and place it in the root of the directory your clone your repos to. That way, you can call it from inside your cloned repo with `../NewRepo.sh`

[Download script](files/NewRepo.sh ':ignore')

[NewRepo.sh](files/NewRepo.sh ':include :type=code')

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

---

Un-stage specific files.

```bash
git reset <file1> <file2>
```

Un-stage all changes.

```bash
git reset
```

---

Stage file for deletion and delete from working area.

```bash
git rm <file1>
```

---

Rename a file, retaining history tracking, staging that change for the next commit.

```bash
git mv <file_name> <new_name>
```

### Branching

Branches allow you to work on multiple features or fixes simultaneously.

List all local branches
```bash
git branch
```

List all branches including remote
```bash
git branch -a
```

---

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

Merge a branch into the current branch
```bash
git merge <branch-name>
```

Delete a branch
```bash
git branch -d <branch-name>
```

---

### PRs (Pull Requests)

Pull Requests are used to review and merge changes from one branch to another, typically from a feature branch to `develop`, or `develop` to `trunk`.

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

Tags are used to mark specific points in your repository’s history, typically for releases.

Create a lightweight tag
```bash
git tag <tag-name>
```

Create an annotated tag
```bash
git tag -a <tag-name> -m "Tag message"
```

---

Push tags to the remote repository
```bash
git push --tags
```

---

### Versioning

Tags can be used for versioning, and git describe can show the most recent tag reachable from a commit.

```bash
git describe --tags --long --dirty
```