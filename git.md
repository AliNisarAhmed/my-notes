# Git Commands

#### Q: git command that forward-ports local commits?

<details>
  <summary>Answer</summary> 
  
  `git rebase`

</details>

#### What does DAG stands for in git context?

<details>
  <summary>Asnwer</summary>

Directed Acyclic Graph - Git commits are DAG's

</details>

---

---

| Command                                                                                         | Summary                                                                                                                         |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `git config --global user.name "Your Name"`                                                     | Add your name to the global Git configuration                                                                                   |
| `git config --global user.email "Your E-mail Address"`                                          | Add your email address to the global Git configuration                                                                          |
| `git config --list`                                                                             | Display all the Git configurations                                                                                              |
| `git config user.name`                                                                          | Display the user.name configuration value                                                                                       |
| `git config user.email`                                                                         | Display the user.email configuration value                                                                                      |
| `git help help`                                                                                 | Ask Git for help about its help system                                                                                          |
| `git help -a`                                                                                   | Print all available Git commands                                                                                                |
| `git --paginate help -a`                                                                        | Paginate the display of all Git commands                                                                                        |
| `git help -g`                                                                                   | Print all available Git guides                                                                                                  |
| `git help glossary`                                                                             | Display the Git glossary                                                                                                        |
| ---                                                                                             | ---                                                                                                                             |
| `git init`                                                                                      | Initialize a Git repository in the current directory                                                                            |
| `git status`                                                                                    | Display status of current directory, as it relates to Git                                                                       |
| `git add FILE`                                                                                  | Start tracking FILE in Git; adds FILE to the staging area                                                                       |
| `git commit -m "MSG"`                                                                           | Commit changes to the Git repository, with a message                                                                            |
| `git log`                                                                                       | Display the log (history) of the Git repository                                                                                 |
| `git log --stat`                                                                                | Display the log with the files that were modified                                                                               |
| `git ls-files`                                                                                  | List the files in the repository                                                                                                |
| ---                                                                                             | ---                                                                                                                             |
| `git diff`                                                                                      | Show any changes between the tracked files in the current directory and the repository                                          |
| `git commit -a -m "Message"`                                                                    | Perform a git add, and then a git commit                                                                                        |
| `git diff --staged` -also- `git diff --cached`                                                  | Show any changes between the staging area and the repository                                                                    |
| `git add --dry-run "path"` -or- `git add -d "path"`                                             | Show what git add would do                                                                                                      |
| `git add .`                                                                                     | Add all new files in the current directory (use git status afterward to see what was added)                                     |
| `git log --shortstat --oneline`                                                                 | Show history using one line per commit, and listing each file changed per commit                                                |
| `git log --oneline`                                                                             | Short one line description of commits. Short form of `git log --pretty=oneline --abbrev-commit`                                 |
| `git reset HEAD <file>`                                                                         | opposite of `git add <file>`, used to unstage an already staged file                                                            |
| `git rm <file>`                                                                                 | Remove file from staging area                                                                                                   |
| `git mv <file1> <file2>`                                                                        | Rename file1 to file2 in the staging area                                                                                       |
| `git add -p`                                                                                    | Pick parts of your changes to add to the staging area                                                                           |
| `git reset <file>`                                                                              | Reset your staging area, removing any changes you’ve added with `git add`                                                       |
| `git checkout <file>`                                                                           | Check out the latest committed version of the file into your working directory                                                  |
| ---                                                                                             | ---                                                                                                                             |
| `git log --parents`                                                                             | Show the history, displaying the parent commit’s SHA1 ID for each commit                                                        |
| `git log --parents --abbrev-commit`                                                             | Same as the preceding command, but shorten the SHA1 ID.                                                                         |
| `git log --oneline`                                                                             | Display history concisely, using one line per each commit.                                                                      |
| `git log --patch`                                                                               | Display the history, showing the file differences between each commit.                                                          |
| `git log --stat`                                                                                | Display the history, showing a summary of the file changes between each commit.                                                 |
| `git log --patch-with-stat`                                                                     | Display the history, combining patch and stat output.                                                                           |
| `git log --oneline file_one`                                                                    | Display the history for file_one.                                                                                               |
| `git rev-parse`                                                                                 | Translate a branch name or a tag name to a specific SHA1 ID.                                                                    |
| `git checkout YOUR_SHA1ID`                                                                      | Change your working directory to match the version specified in YOUR_SHA1ID.                                                    |
| `git tag TAG_NAME -m "MESSAGE" YOUR_SHA1ID`                                                     | Create a tag named TAG_NAME, pointing to YOUR_SHA1ID (or a relative ref). The tag will have a short MESSAGE associated with it. |
| `git tag`                                                                                       | List all tags.                                                                                                                  |
| `git show TAG_NAME`                                                                             | Show information about the tag named TAG_NAME.                                                                                  |
| `git log --reverse`                                                                             | List history in reverse                                                                                                         |
| `git log -N`                                                                                    | Limits the git log output to N commits, where N is any number. Can also be written as `-n N` or `--max-count=N`                 |
| `git log --relative-date`                                                                       | display the date as time relative to the current time                                                                           |
| `git commit --amend -m "Message"`                                                               | Change last git commit message                                                                                                  |
| `git rev-parse :/"SEARCH_STRING"`                                                               | will search the commit logs, and return SHA1 ID of the commit that contains the SEARCH_STRING                                   |
| `git rev-parse <tag>`                                                                           | get SHA1 ID of the <tag>                                                                                                        |
| `git tag -d <tag>`                                                                              | delete <tag>                                                                                                                    |
| `git log --author=ali`                                                                          | search the logs where author == ali                                                                                             |
| ---                                                                                             | ---                                                                                                                             |
| `git branch`                                                                                    | List all branches.                                                                                                              |
| `git branch dev`                                                                                | Create a new branch named dev. (This branch points to the same commit as HEAD.)                                                 |
| `git checkout dev`                                                                              | Change your working directory to the branch named dev.                                                                          |
| `git branch -d master`                                                                          | Delete the branch named master.                                                                                                 |
| `git log --graph --decorate --pretty=oneline --all --abbrev-commit`                             | View history of the repository across all branches.                                                                             |
| `git config --global alias.lol "log --graph --decorate --pretty=oneline --all --abbrev-commit"` | Make an alias named `lol` for the git log                                                                                       |
| `git branch -v`                                                                                 | List all branches with SHA1 ID information.                                                                                     |
| `git branch fixing_readme YOUR_SHA1ID`                                                          | Make a branch using YOUR_SHA1ID as the starting point.                                                                          |
| `git checkout -b another_fix_branch fixing_readme`                                              | Make a branch named another_fix_branch using branch fixing_readme as the starting point.                                        |
| `git reflog`                                                                                    | Show a record of all the times you changed branches (using `checkout`)                                                          |
| `git stash`                                                                                     | Set the current work in progress (WIP) to a stash (holding area), so you can perform a git checkout.                            |
| `git stash list`                                                                                | List works in progress that you’ve stashed away.                                                                                |
| `git stash pop`                                                                                 | Apply the most recently saved stash to the current working directory; remove it from the stash.                                 |
| `git check-ref-format --help`                                                                   | Ensures that a reference name is well formed                                                                                    |
| `git checkout -b <new branch> <starting point>`                                                 | create a new branch @ <starting point> and switch to that branch                                                                |
| `git branch -m <existing branch> <new branch name>`                                             | Rename existing branch to new branch name                                                                                       |
| `git branch --contains <tag>`                                                                   | Only list branches which contain the speicifed commit or tag                                                                    |
| ---------------------------------                                                               | ----------------------------------                                                                                              |
| `git diff BRANCH1...BRANCH2`                                                                    | Indicate the difference between BRANCH1 and BRANCH2 relative to when they first became different.                               |
| `git diff --name-status BRANCH1...BRANCH2`                                                      | Summarize the difference between BRANCH1 and BRANCH2, by listing each file and its status.                                      |
| `git merge BRANCH2`                                                                             | Merge BRANCH2 into the current branch that you’re on.                                                                           |
| `git log -1`                                                                                    | A shorthand for git log -n 1 (show only the most recent commit).                                                                |
| `git mergetool`                                                                                 | Open a tool to help perform a merge between two conflicted branches.                                                            |
| `git merge --abort`                                                                             | Abandon a merge between two conflicted branches.                                                                                |
| `git merge-base BRANCH1 BRANCH2`                                                                | Show the base commit between BRANCH1 and BRANCH2.                                                                               |
| -------                                                                                         | ------                                                                                                                          |
| `git clone source destination_dir`                                                              | Clone the Git repository at source to the destination_dir.                                                                      |
| `git log --oneline --all`                                                                       | Display all commit log entries from all branches. (Normally, git log displays only entries from the current branch.)            |
| `git log --simplify-by-decoration --decorate --all --oneline`                                   | Display the history in a simplified form.                                                                                       |
| `git branch --all`                                                                              | Show remote-tracking branches in addition to local branches.                                                                    |
| `git clone --bare source destination_dir`                                                       | Clone the bare directory of the source repository into the destination_dir. By convention destination_dir should end with .git. |
| `git ls-tree HEAD`                                                                              | Display all the files for HEAD (the current branch).                                                                            |
| ----------                                                                                      | --------                                                                                                                        |
| `git checkout -f master`                                                                        | Check out the master branch, throwing away any changes in your current branch.                                                  |
| `git remote`                                                                                    | Display the name of the remote(s) in the current repository.                                                                    |
| `git remote -v`                                                                                 | show Display the names of the remotes along with the corresponding remote URL.                                                  |
| `git remote add bob ../math.bob`                                                                | Add a remote named bob that points to the local repository in ../math.bob.                                                      |
| `git ls-remote REMOTE`                                                                          | Display the references of a remote repository (use . as the REMOTE when you want the current local repository).                 |
| `GIT_TRACE_PACKET=1 git ls-remote`                                                              | REMOTE Display the underlying network interaction.                                                                              |
| `git remote remove <remote name>`                                                               | Remove a remote named <remote name>                                                                                             |
| `git remote set-url <remote name> <path to new url>`                                            | Change the url of the <remote name> to <path to new url>                                                                        |
| `git remote rename <old remote name> <new remote name>`                                         | Change the name of a remote                                                                                                     |
