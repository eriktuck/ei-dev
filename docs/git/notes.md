HELP ME PLEASE!

## Setting Up Git & GitHub

### Download Git

git-scm.com
Download & run installer
`git --version` on cmd to make sure its successfully installed

### Configure Git

`git config --global user.name <name>` 
`git config --global user.email <email>`
`git config --list` to see all settings

**Git Log**
`git log` <- all commits
`git show` <- last commit 
`git ls-files` <- lists all files that git is tracking
`git log --oneline --graph --decorate --all` <-more detailed history of commits

##### To create an alias for the git history command
`git config --global alias.hist "log --oneline --graph --decorate --all"`
now use git `hist` to see the same command, note it still accepts additional arguments (for example, provide `--filename` to see history for one file)

History will be served line by line, type `q` to quit at any time

**Setup editor** (VSCode should be already available as 'code')
Add full folder path that includes executable to system path environment variable, separate with semi-colon
Restart Bash
Create alias for editor
`notepad++ ~/.bash_profile`
`alias npp='notepad++ -mulitInst - nosession'`
`git config --global core.editor "notepad++ -multiInst -nosession"`
`git config --global -e`

**Setup Diff & Merge Tool**

This section describes how to use p4merge. Use VS Code instead

git config --global diff.tool p4merge
git config --gloabl difftool.p4merge.path "C:/... /p4merge.exe
git config --global difftool.prompt false
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "C:/.../p4merge.exe
git config --global mergetool.prompt false

## Git Workflow

### Common workflow on master

1. Navigate to root project folder in Windows Explorer
2. Right-click and select 'Git Bash Here' from context menu
3. `git status`
   1. commit and push any changes if found, but usually wouldn't be
4. Work on a feature
   1. You can open, add, and remove files using the Windows Explorer GUI or using unix commands in Git Bash
5. Commit changes locally
   1. If new files were added, `git add -A` then `git commit -m "Message"`
   2. Otherwise, express commit `git commit -am "Message"`
6. Push to GitHub
   1. `git pull origin master` in case others are working on it
   2. `git push origin master` 

### Common workflow on branch

#### Creating and committing a branch

`git branch <name>` <- creates branch
`git checkout <branch name>` <- checks out 
make changes
`git add -A`
`git commit -m "Message"`
`git push -u origin <branch name>` (first time only)
`git branch -a` (to confirm)

#### Merging to master

`git checkout master`
`git pull origin master`
`git branch --merged` (which branches have been merged?)
`git merge <branch name>`
`git push origin master`
`git branch -d <branch name>` (to delete branch locally)
`git push origin --delete <branch name>` (to delete the remote branch)

#### Workflow Commands

Adding files to staging area
Use `git add <filename>` to add file
Use `git add -A` to add everything in the working directory
(Use `git reset <filename>` to remove from the staging area or git reset to remove everything)

Regular commit 
`git commit -m "Message"`
If `-m "Message"` is excluded, the default editor will be opened and a message should be inputted. No need to add quotes or anything else. Line breaks will be ignored.

Express commit
`git commit -am "Message"`
For any files already tracked (use `git ls-files` if unsure or `git status` to see what's in staging area)

Renaming files
`git mv <file1> <file2>`
`git commit -m "message"`
also can use `git add -A` if changes (renames, deletions) made outside of git

Delete files (from git tracking)
`git rm filename`
`git commit -m "message"`
also can use `git add -u` if files deleted outside of git

Cloning remote repositories
`git clone <url>`
`git remote -v` <- shows remote connection
`git branch -a` <- shows branches in repository

Pushing
`git diff` <- shows changes

[Example workflow](https://www.gun.io/blog/how-to-github-fork-branch-and-pull-request)

### Other Common Commands

**Help** `git <action> --help`

**Remove file after adding to .gitignore**

```bash
git rm -r --cached <file or folder name>
git commit -m "Removed files message"
git push origin master
```

**List files** `git ls-files`

**Open file with default editor** `start <file name>`

**Open file with VSCode** `code <file name>` (set up default editors with aliases or use `start` and ensure preferred editor is system default)

### Diffs & Merges

If you encounter a merge issue after pulling from the repo, and it can't be automatically merged, open the file in VSCode and accept/reject changes (will be highlighted in VSCode).

`code <mergefile>`

Then save and close the editor. You can now add the file to the staging area and push all changes to the repo (`git commit -m "Fixed merge conflict"` > `git push`).

### Working Together

There are two common ways of collaborating:

1. Fork the repo and submit pull requests to the repo owner
2. Add Collaborators to your repo to give others push authority

Forking the repo and submitting pull requests is the safest, as the repo owner is in charge of reviewing all proposed changes before integrating them into the repo. However, that can create a lot of work for the repo owner depending on the frequency of commits.

Adding Collaborators can be done in the Settings tab of a repo. This allows anyone listed as a collaborator to work on the repo as if it was their own. This will streamline the workflow, but you risk missing simple mistakes, severe mistakes, and malicious intent. 

https://kbroman.org/github_tutorial/pages/fork.html

### Removing files 

If you want to remove a file that has already been committed:

1. Add the file to the .gitignore file
2. Use the command `git rm --cached <filename>`

### Adding empty folders

Empty folders are automatically ignored by git, since it only stores files. Sometimes you want to include an empty folder if you want to commit a folder structure before populating it with files or add a folder for temporarily storing data. To do this you need to add an empty file to the folder. By convention, we can call this `.keep` or `.gitkeep`. Create a new text document within the folder you want to keep, leave it empty, and save it as `.keep` or `.gitkeep`.

If you also want to ignore the contents of this folder, but keep the folder (e.g., for log files or temporary data that isn't deleted), add the following to the `.gitignore` file:

```
# ignore the files in the folder foo
foo/*

# but keep the folder
!foo/.keep
```

