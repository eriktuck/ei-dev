## EI Development Guidance

This was produced with mkdocs. For full documentation visit [mkdocs.org](https://mkdocs.org).

## Contents

* Home
* Portfolio
* Metrics Services
* Metrics Design Philosophy
* Project Planning
* Git
* Development
* Deployment (non-tech: how do we deliver products?)
* Data Management (non-tech: database overview)
* Data Science (non-tech: types of data analysis)
* Data Visualization (non-tech: visualization options)
* Spatial Analysis (non-tech: types of spatial data analysis)
* Packages
* News/Blog 
* How to Maintain this Site
* Additional Resources

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

## Installations

`mkdocs` and the `material` theme must be installed:

* `pip install mkdocs`

* `pip install mkdocs-material`

## Contributing

This project is available on GitHub [here](https://github.com/eanderson-ei/ei-dev).

Fork the project to begin contributing. You can find more information on how to fork a project [here](../git/collaborating-with-git.md), or if you're unfamiliar with Git and GitHub, start [here](../git/install-configure-git.md).

Here are the technologies used in this project, you'll need to be familiar with these to contribute.

* MkDocs - static site generator that requires Markdown
* Markdown - markup text language
* Typora - Markdown editor
* VS Code - IDE for yaml
* Git - version control
* Github - code sharing
* Github Project Pages - Deployment (gh-pages branch); see [MkDocs deployment documentation](https://www.mkdocs.org/user-guide/deploying-your-docs/).
* Screen2Gif - a screen recording app that saves outputs as gifs (for video instruction)
* YouTube - custom videos for instruction, etc.

Use `mkdocs serve` to visualize the site while working on it.

Add the `site/` folder to the .gitignore file.

### Deploying changes

To deploy the website:

1. Open the local repository
2. Make sure you're working on the master branch (use `git status`)
3. Open the Anaconda Prompt* and run

```bash
mkdocs gh-deploy
```

Note that in the GitHub repo on the Settings Tab, under project pages, should have the ghp-pages branch selected as the page.

*To add the Anaconda Prompt to your right-click context menu on windows, see this [gist](https://gist.github.com/jiewpeng/8ba446acf329b1801bf91db767d179ea).

#### Customs Used

To identify a user-provided value, wrap it in `<>` within a code block:	`git commit <updated-file>`

Highlight package names, files, code, and cli commands in code blocks: `rasterio`