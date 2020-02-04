## Git Workflow

Once you work with Git a few times, you'll have this workflow memorized. See the next section for more on [Collaborating with Git], and the [Additional Reference] if you don't find what you need here.

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
git branch <branch_name>  # creates branch
git checkout <branch_name>  # checks out branch
# work on your feature
git add -A
git commit -m "<message>"
git push -u origin <branch_name>  # the -u flag is required only on first commit
git branch -a  # to confirm
```

#### Merging to master

After you've tested the branch, you will need to merge with master.

```bash
git checkout master  # move from working on branch to master
git pull origin master  # update master branch
git branch --merged  # prints out which branches merged
git merge <branch_name>  # merges branch with master
git push origin master
git branch -d <branch_name>  # delete branch locally
git push origin --delete <branch_name>  # delete branch remotely
```

## Workflow Commands

#### Adding files to staging area

Use `git add <filename>` to add a file.
Use `git add -A` to add everything in the working directory.

#### Removing files from the staging area

Use `git reset <filename>` to remove from the staging area or `git reset` to remove everything.

#### Regular commit

`git commit -m "Message"`
If `-m "Message"` is excluded, the default editor will be opened and a message should be inputted. No need to add quotes or anything else. Line breaks will be ignored.

#### Express commit

`git commit -am "Message"`
For any files already tracked (use `git ls-files` if unsure or `git status` to see what's in the staging area).

#### Renaming files

`git mv <file1> <file2>`
Use `git add -A` if changes (renames, deletions) were made outside of Git. Git is smart enough to know if a new file in the staging area is actually a renamed file that was deleted.

#### Delete files (from Git tracking)

`git rm filename`
Use `git add -u` if files were deleted outside of Git Bash.

#### Removing files from Git history

If you want to remove a file that has already been committed:

1. Add the file to the `.gitignore` file
2. Use the command `git rm -r --cached <filename>`

#### Diffs

Use `git diff <filename>` to shows recent changes to a file.

[Example workflow](https://www.gun.io/blog/how-to-github-fork-branch-and-pull-request)

## Other Common Commands

**Help** `git <action> --help`

**List files** `git ls-files`

**Open file with default editor** `start <file name>`

**Open file with VSCode** `code <file name>` 

### Diffs & Merges

If you encounter a merge issue after pulling from the repo, and it can't be automatically merged, open the file in VSCode and accept/reject changes (will be highlighted in VSCode).

`code <mergefile>`

Then save and close the editor. You can now add the file to the staging area and push all changes to the repo (`git commit -m "Fix merge conflict"` > `git push`).

### Working Together

There are two common ways of collaborating:

1. Fork the repo and submit pull requests to the repo owner
2. Add Collaborators to your repo to give others push authority

Forking the repo and submitting pull requests is the safest, as the repo owner is in charge of reviewing all proposed changes before integrating them into the repo. However, that can create a lot of work for the repo owner depending on the frequency of commits.

Adding Collaborators can be done in the Settings tab of a repo. This allows anyone listed as a collaborator to work on the repo as if it was their own. This will streamline the workflow, but you risk missing simple mistakes, severe mistakes, and malicious intent. 

https://kbroman.org/github_tutorial/pages/fork.html

### Unstaging files



2. 

# Additional Reference

### Adding empty folders

Empty folders are automatically ignored by git, since it only stores files. Sometimes you want to include an empty folder if you want to commit a folder structure before populating it with files or add a folder for temporarily storing data. To do this you need to add an empty file to the folder. By convention, we can call this `.keep` or `.gitkeep`. Create a new text document within the folder you want to keep, leave it empty, and save it as `.keep` or `.gitkeep`.

If you also want to ignore the contents of this folder, but keep the folder (e.g., for log files or temporary data that isn't deleted), add the following to the `.gitignore` file:

```
# ignore the files in the folder foo
foo/*

# but keep the folder
!foo/.keep
```

