## Git Workflow

Once you work with Git a few times, you'll have this workflow memorized. See the next section for more on [Collaborating with Git](collaborating-with-git.md), and the [Additional Reference](additional-reference.md) if you don't find what you need here.

### Common workflow on master

The master branch is the main branch of the project. When you're working on your own project without collaborators or your project is not very complex, it's common to work on master. Otherwise, you'll want to [work on a branch](#common-workflow-on-branch).

1. Navigate to root project folder in Windows Explorer.
2. Right-click and select 'Git Bash Here' from context menu.
3. Type `git status`: commit and push any changes if found (see next steps), but there shouldn't be any since you'll always commit and push all changes at the end of each session.
4. Pull any changes from GitHub with `git pull origin master`.
5. Work on your feature. You can open, add, and remove files using the Windows Explorer GUI or using unix commands in Git Bash.
6. Commit changes locally:
    1. If new files were created, `git add -A` then `git commit -m "Message"`
    2. Otherwise, express commit `git commit -am "Message"`
7. Push to GitHub:
    1. `git pull origin master` (in case others are working on it)
    2. `git push origin master` 

If you work on multiple features in one session without committing changes, you might want to commit each file one at a time so that your commit messages are associated with just the relevant changes. Use `git status` to see where you have changes, and use `git add <filename>` to add only the relevant files to the staging area to commit, commit those changes, and then add the next file or set of files. You can pass in folders (which will include all files within the folder) or globbing patterns (i.e., wildcards) to select multiple files at once. You can wait until all changes are committed to push your changes to GitHub. 

##### Messages

Git messages typically are written as directives, rather than in past tense, such as 'rename dataframe columns'.

#### Summary

Here's that list of commands again:

```bash
git status
git pull origin master
# work on your feature
git add -A 
git commit -m "<message>"
git pull origin master
git push origin master
```

### Common workflow on branch

Now that you're familiar with working on master, we'll quickly summarize the standard workflow for working on a branch. 

#### Creating and committing a branch

```bash
git checkout -b <branch_name>  # creates new branch and switches to it
# work on your feature
git add -A
git commit -m "<message>"
git push -u origin <branch_name>  # the -u flag is required only on 
```

#### Working on a branch

Use `git branch -a`  to see all branches.

```bash
git checkout <branch_name>
# work on your feature
git add -A
git commit -m "<message>"
git push origin <branch_name>
```

#### Merging to master

After you've tested the branch, you will need to merge with master.

```bash
git checkout master  # move from working on branch to master
git pull origin master  # update master branch
git merge <branch_name>  # merges branch with master
git push origin master
git branch -d <branch_name>  # delete branch locally
git push origin --delete <branch_name>  # delete branch remotely
```

## Workflow Commands

#### Add files to staging area

Use `git add <filename>` to add a file.
Use `git add -A` to add everything in the working directory.

#### Remove files from the staging area

Use `git reset HEAD <filename>` to remove from the staging area or `git reset HEAD` to remove everything.

#### Regular commit

`git commit -m "Message"`
If `-m "Message"` is excluded, the default editor will be opened and a message should be inputted. No need to add quotes or anything else. Line breaks will be ignored.

#### Express commit

`git commit -am "Message"`
For any files already tracked (use `git ls-files` if unsure or `git status` to see what's in the staging area).

#### Ammend last commit message

`git commit --amend -m "<ammended commit message"`

#### Rename files

`git mv <file1> <file2>`
Use `git add -A` if changes (renames, deletions) were made outside of Git. Git is smart enough to know if a new file in the staging area is actually a renamed file that was deleted.

#### Delete files (from Git tracking)

`git rm filename`
Use `git add -u` if files were deleted outside of Git Bash.

#### Remove files from Git history

If you want to remove a file that has already been committed:

1. Add the file to the `.gitignore` file
2. Use the command `git rm -r --cached <filename>`

#### Undo changes

Use `git checkout -- <filename>` to undo the changes made since the last commit.

#### Compare differences

Use `git diff <filename>` to shows recent changes to a file.

Use `git diff <commit1> <commit2>` to compare all changes between two commits (use `git hist` to git the sha-1 codes for commits). Use `HEAD` as the second sha-1 hash to compare to the last commit.

#### For a more comprehensive overview, see here

[Example workflow](https://www.gun.io/blog/how-to-github-fork-branch-and-pull-request)

## Other Common Commands

**Help** `git <action> --help`

**List files** `git ls-files`

**Open file with default editor** `start <file name>`

**Open file with VSCode** `code <file name>` 

## Merges

If you encounter a merge issue after pulling from the repo, and it can't be automatically merged, use VSCode (your merge tool) to address any conflicts. 

While Git is in a merging state, launch your merge tool with `git mergetool`. Git will open each conflicting file in VSCode one at a time. Conflicting code will be highlighted. Address conflicts, save and close the editor. You can now add the file to the staging area and push all changes to the repo. 

You might find a new file with `.orig` extension created by Git after this process. This is the original version of the conflicting file. Delete this file using `git rm <filename.orig>`. You might also want to add this file extension to your `.gitignore` file.

## Tags

Git supports tagging to mark major milestones in your repository. Say you're finally ready to deploy, and you want to make sure you know the version of your project that is getting deployed. You could then create a tag each time the code is deployed. 

Use `git tag -a <tag_name> -m "<tag message>"` to create a tag. For example, you could tag v1.0 of your code using `git tag -a v1.0 -m "Version 1.0 Release"`.

To see tags, use `git tag --list`. You can also see tags in context of your history with `git hist`. If you want to see the details associated with a specific tag, use `git show <tag_name>`.