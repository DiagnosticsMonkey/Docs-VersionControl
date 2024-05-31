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

> Save the script as `NewRepo.sh` and place it in the root of the directory your clone your repos to. That way, you can call it from inside your cloned repo with `../NewRepo,sh`

```bash
#!/bin/bash

# Colour Codes
RED='\033[91m'
GREEN='\033[92m'
YELLOW='\033[93m'
BLUE='\033[94m'
PURPLE='\033[95m'
CYAN='\033[96m'
# Styles
UNDERLINE='\033[4m'
BOLD='\033[1m'
RESET='\033[0m'

# Script Variables
NewLn="\r\n"
Author="TAKTAK"
ScriptName="${0##*/}"
ExUsage="Usage: After cloning an empty repo, run this script from inside the repo.${NewLn}   Example: ../${ScriptName}"

# --------------

# Functions for Error and Warning Handling
print_header(){
   echo -e "${PURPLE}${BOLD}$1${RESET}"
}
print_fatal_error() {
   echo -e "${RED}${BOLD}Fatal Error: $1${RESET}"
   exit 1
}
print_error() {
   echo -e "${RED}${BOLD}Error: $1${RESET}"
}
print_warning() {
   echo -e "${YELLOW}${BOLD}Warning: $1${RESET}"
}
print_success() {
   echo -e "${GREEN}${BOLD}$1${RESET}"
}
print_info() {
   echo -e "${BLUE}$1${RESET}"
}
print_separator() {
   echo -e "${CYAN}------------------------------------${RESET}"
}

# --------------

# We sudoed?
check_sudo() {
   if [ "$EUID" -ne 0 ]; then
      print_fatal_error "Permissions you don't have; sudo you must."
   fi
}

# --------------

main() {
   # Author and Usage
   print_header "Script by: ${Author}${NewLn}   ${ExUsage}"
   #print_usage "${ExUsage}"

   # Do stuff
   if ! git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
      print_fatal_error "You have not run this script in a Git repo."
   fi

   # Grab branches
   branches=$(git branch)

   # Check for empty
   if [ -z "${branches}" ]; then
      # No branches found, so make some
      print_info "No branches found. Continuing."
      git checkout -b trunk
      echo "# ReadMe" > README.md
      git add README.md
      git commit -m "Initial commit"
      git push -u origin trunk
      git checkout -b develop
      git push -u origin develop
      git tag -a v0.0.0 -m "Epoch tag"
      git push --tags
      print_info "Branches created, epoch tag set."
   else
      # Bang out
      print_warning "Pre-existing branches found:${NewLn}${branches}"
      print_fatal_error "You should only run this script on an empty repo."
   fi

   print_success "Script reached the end."
}

main "$@"

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