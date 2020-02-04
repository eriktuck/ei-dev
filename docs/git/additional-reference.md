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

### Git fetch

`git pull` is in fact two commands, `git fetch` and `git merge`. If you have changes in a remote repository that are conflicting with your local repository `git pull` can be destructive to your changes. You can use `git fetch` to bring down the remote changes and `git status` to see what has changed. You can then use `git pull` to merge changes after you have reviewed them. Some people have strong opinions about whether `git fetch` is best practice, or if `git pull` is just fine.

### Prune

`git fetch -p` will prune (delete) stale references if you made changes on GitHub that aren't reflected in your local repository, such as deleting a branch.

### Transfer project

Repositories can be transferred from a personal accont to an organization (or vice versa). Under the repo settings, in the 'Danger Zone', select transfer. You might then want to fork the repo back to the original account.

### TO DO

* Rebase
* Organizations & Teams