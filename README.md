# ei-dev
Documentation for standard EI development practices

View at https://eanderson-ei.github.io/ei-dev/

To push changes:

1. Open the local repository
2. Make sure you're working on the master branch (run `git status`)
3. Open the Anaconda Prompt* and run
```bash
mkdocs gh-deploy
```
Note that the Settings Tab, under project pages, should have the ghp-pages branch selected as the page.

Run  `mkdocs serve` to visualize the site while working on it.

Add the `site/` folder to the .gitignore file because it doesn't need to be served up to github.

*To add the Anaconda Prompt to your right-click context menu on windows, see this [gist](https://gist.github.com/jiewpeng/8ba446acf329b1801bf91db767d179ea).
