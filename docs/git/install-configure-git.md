# Installing & Configuring Git

Git is version control software for code. When paired with Github, Git is a convenient way to collaborate on coding projects. This series will help you get comfortable with the basic workflow and commands, and includes a helpful [reference] for when you get stuck.

### Download & Install Git

Go to [git-scm.com](git-scm.com) to download Git. Run the installer. I recommend installing only Git Bash in the context menu. I also recommend checking the option to '*Use Git from the Windows Command Prompt*'. 

Open Git Bash (right-click to bring up the context menu or find it in your programs folder) and type  `git --version` to make sure it's successfully installed.

### Configure Git

To get the most from Git, you can configure a few of its global setings. In Git Bash, type the following commands:

```bash
git config --global user.name <name> 
git config --global user.email <email>
```


Replace `<name>` and `<email>` with your name and email. Your name and email will be associated with any commits you make (i.e., when you save and publish edits to code). Use `git config --list` to see all settings available to you.

#### Git Log

The log is helpful when you need to see the history of changes made or roll back to a previous change. These commands help you navigate the logs. 

`git log` | all commits
`git show` | last commit 
`git ls-files` | lists all files that git is tracking
`git log --oneline --graph --decorate --all` |more detailed history of commits

#### Create an alias for the git history command

We'll customize the `hist` command to provide more information when we use it. You can use this pattern to alias other commands as well. 

In Git Bash, type:

```bash
git config --global alias.hist "log --oneline --graph --decorate --all"
```

Now use `git hist` to see the same command, note it still accepts additional arguments (for example, use `git hist <filename>` to see history for one file).

History will be served line by line, type `q` to quit at any time. 

#### Setup your preferred editor 

You must provide a message describing your changes when you commit. You can usually type this directly into Git Bash, but sometimes you'll want to provide a lengthier message. You can set up your default editor for commit messages. This will set up VSCode as the default editor (note that `--wait` is only required for VSCode, and other editors may not require that flag).

```bash
git config --global core.editor "code --wait"
```

Type `git config --global -e` to confirm this worked. VSCode should open and display the contents of Git's configuration file.

!!! info
    You can open files with the command `start <filepath>` using your system's default editor for that filetype. If you want to open a project in VSCode from the command line (or Git Bash), type `code .`, where the period represents the current folder. This will open VSCode within the context of the project's folder. See the [documentation](https://code.visualstudio.com/docs/editor/command-line) for other ways you can open files/folder from the command line.

**Setup Diff & Merge Tool**

When there are conflicting changes to a file, you'll need to tell git which changes to keep using Diff and Merge tools. Type the following one line at a time in Git Bash. [REQUIRES TESTING]


```bash
git config --global diff.tool vscode
git config --global difftool.vscode cmd = "code --wait $MERGED"
git config --global difftool.prompt false
git config --global merge.tool p4merge
git config --global mergetool.vscode cmd = "code --wait --diff $LOCAL $REMOTE"
git config --global mergetool.prompt false
```

!!! info
    You can also use VSCode to compare two files using the command `code --diff <filepath1> <filepath2>`. This option is available from within VSCode through the context menu as well.