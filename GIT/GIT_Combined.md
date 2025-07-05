# GIT Combined Notes

## 001.GIT-why-use-git.md
✨ **Why Use Git**
- → Git is a distributed version control system (DVCS).
- → The entire project is downloaded to the local machine, allowing offline work.
- → It is free and open source.
- → It is fast and lightweight due to working with a local repository.
- → It is very secure, using secured hashing mechanisms for communication between local and remote repositories.
- → Makes pushing/pulling changes and creating branches super simple.

## 002.GIT-what-is-the-typical-git-workflow.md
✨ **Typical Git Workflow**
- → **Start:** Either create a new remote repository or clone an existing one.
- → **Local Copy:** Cloning a project creates a local copy on your machine.
- → **Make Changes:** Modify files in your local repository.
- → **Stage Changes:** Add modified files to the staging area (`git add`).
- → **Commit Changes:** Commit staged changes (`git commit`). A new commit ID is created, and the HEAD always points to the latest commit.
- → **Push Changes:** Push committed changes to the remote repository (`git push`) to share with other developers.
- → **Pull Changes:** Use `git pull` or `git fetch` to get the latest changes from the remote repository to your local repository.

## 003.GIT-commands-you-have-used.md
✨ **Common Git Commands**
- → `git status`: Checks the status of the local repository and files.
- → `git add <file>` / `git stage <file>`: Adds files to the staging area.
- → `git commit -m "<message>"`: Commits staged changes with a message.
- → `git push`: Pushes committed changes to the remote repository.
- → `git log`: Shows all commits.
- → `git diff <commit_id1> <commit_id2>`: Shows differences between two commits.
- → `git reset`: Reverts changes in the staging area or committed changes (removes commits from history).
- → `git revert <commit_id>`: Creates a new commit that undoes the changes of a specified commit (preserves history).
- → `git branch`: Shows the current branch.
- → `git checkout <branch_name>`: Switches to a different branch.

## 004.GIT-tools-you-have-used.md
✨ **Git Tools Used**
- → **Command Line:** Primarily used the Git command line.
- → **IDE Support:** Used built-in Git support in IDEs like Spring Tool Suite and IntelliJ IDEA (UI-based tools).
- → **Git Clients:** Worked with SourceTree (a Git client tool).
- → **GitHub Desktop:** Used for its simplicity.

## 005.GIT-fetch-vs-pull.md
✨ **Fetch vs Pull**
- → **`git fetch`:**
    - → Retrieves only the list of changes and differences that happened in the remote repository.
    - → It does not fetch the actual changed files to the local repository.
- → **`git pull`:**
    - → Fetches the actual changes (files) from the remote repository to the local repository on your machine.
    - → It is essentially a `git fetch` followed by a `git merge`.

## 006.GIT-reset-and-revert.md
✨ **Reset vs Revert**
- → **`git reset`:**
    - → Allows resetting the HEAD to a particular commit.
    - → All subsequent commits after the target commit are effectively removed from the history.
    - → This means those changes are gone from the commit history.
- → **`git revert`:**
    - → Creates a *new commit* that undoes the changes of specified commits.
    - → It does not remove the original commits from the history.
    - → If you revert commits 4 and 5 to go back to the state of commit 3, it creates a new commit that undoes the changes of 4 and 5, and the HEAD points to this new commit.
    - → Preferred over `reset` when you want to undo changes while preserving the commit history.
