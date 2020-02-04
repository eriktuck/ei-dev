# Collaborating with Git

There are two common ways of collaborating:

1. [Fork the repo](#fork-the-repo) and submit pull requests to the repo owner
2. [Add collaborators](#add-collaborators) to your repo to give others push authority

### Fork the repo

Forking the repo and submitting pull requests is the safest, as the repo owner is in charge of reviewing all proposed changes before integrating them into the repo. However, that can create a lot of work for the repo owner depending on the frequency of pull requests.

### Add collaborators

Adding collaborators can be done in the Settings tab of a repo (Settings > Collaborators) . This allows anyone listed as a collaborator to work on the repo as if it was their own. This will streamline the workflow, but you risk missing simple mistakes, severe mistakes, and malicious intent. 

https://kbroman.org/github_tutorial/pages/fork.html

## Working on someone else's project

If you are not a collaborator, you will need to fork the project to your GitHub.

1. Navigate to the repository on GitHub
2. Select 'Fork', which will copy the project to your own repository
3. Clone your version of the repo to your computer using `git clone <project>` in Git Bash (see [Initializing Git](initializing-git.md) for help here)
4. Work [as you normally would](common-workflow.md), working on a branch rather than master. *Be sure to always pull changes before working on your feature AND before pushing changes.*
5. Use `git push -u origin <branch_name>` to push changes the first time, otherwise `git push origin <branch_name>`. This will push the changes to your GitHub repository.
6. On GitHub, click the green 'Compare and Pull Request' button to open a pull request. Provide a comment that describes the request.
7. Click 'Create Pull Request'. The  target repository will open up in your browser.
8. Update the Pull Request if needed.

You can continue adding commits to this pull request by working on this branch, if you want. You may be requested by the project owner to make changes to your code before the pull request is accepted, which can be done by working on this branch.

### Working as a collaborator

As a collaborator, you will be able to work on this project as if it were your own. Clone the project to your computer and push changes as you normally would.

## Accepting pull requests

As the project owner, you'll receive changes as pull requests from others working on your project. GitHub allows you to comment on the request, review the commits, and see files changed. You can even provide in-line code comments when reviewing files changed.

GitHub will display whether the pull request can be merged automatically (i.e., there are no conflicting changes). Once you're satisfied with the changes, click 'Confirm Merge' to accept the changes. You can revert the changes until you refresh or navigate away from the page.

Delete branches after they have been merged successfully.

You can also set up a version of the other repo as a remote connection `git remote add <remote_name> <remote location>` and simply pull changes `git pull <remote_name> master` then push back to your repository `git push origin master`. 

See more [here](https://kbroman.org/github_tutorial/pages/fork.html).

## Merges

When working with others, you'll be merging their changes to your files whenever you use `git pull`. Git has three flavors of merges.

* **Fast forward** merge: when there are no changes in parent branch, the commits are added as if you were working on the parent branch the entire time.
* **Automatic** merges: when there are no conflicting changes in parent branch, a new merge commit will be created and you will be asked for a message for this commit.
* **Manual** merge: when Git can't resolve conflicting changes it moves into merging state. You must resolve conflicts before proceeding. If you've configured VSCode as your merge tool, you will be prompted to address the conflicting changes one file at a time.

Ideally, you'll be communicating with your team members to avoid creating conflicting changes that require manual merges.