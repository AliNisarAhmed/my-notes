| command                                           | function                                                                     |
| ------------------------------------------------- | ---------------------------------------------------------------------------- |
| `git config --list`                               | Display all the git configurations                                           |
| `git config user.name`                            | display user name                                                            |
| `git config user.email`                           | display email                                                                |
| --                                                | --                                                                           |
| `git help -a`                                     | Print all available git comamnds                                             |
| `git --paginate help -a`                          |                                                                              |
| `git help -g`                                     |                                                                              |
| `git help glossary`                               |                                                                              |
| --                                                | --                                                                           |
| `git add <file>`                                  | add files to the staging area and starts tracking                            |
| `git add --dry-run <path>` or `git add -d <path>` | Tells which files will be added if run.                                      |
| `git add -p`                                      | Pick parts of the changes to add to the staging area                         |
| --                                                | --                                                                           |
| `git log`                                         | list all the commits                                                         |
| `git log --stat`                                  | Log the commits, and the files that are part of the commit                   |
| `git log --oneline`                               | list the commits in one line                                                 |
| `git log --pretty=oneline --abbrev-commit`        | SAME AS ABOVE                                                                |
| `git log --shortstat --oneline`                   | Show history using one line per commit, listing each file changed per commit |
| `git ls-files`                                    | list the files in the repository                                             |
| --                                                | --                                                                           |
| `git diff`                                        | Show any changes between the working area & the staging area                 |
| `git diff --staged`                               | Show any changes between the staging area and the repo                       |
| --                                                | --                                                                           |
| `git commit -m <message>`                         | commit with a message                                                        |
| `git commit -a -m <message>`                      | commit all files with a message                                              |
| --                                                | --                                                                           |
| `git reset <FILE>`                                | Reset the Staging area, removing any changes added by `git add`              |
| --                                                | --                                                                           |
| `git checkout <FILE>`                             | Check out the latest committed verison of FILE into your working directory   |
| --                                                | --                                                                           |
| `git rm <FILE>`                                   | Remove FILE from the Staging area                                            |
| --                                                | --                                                                           |
| `git mv <FILE1> <FILE2>`                          | Rename FILE1 to FILE2 in the staging area                                    |
