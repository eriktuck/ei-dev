## Getting Started 

Before beginning any project, start by thinking it through. What technologies are required? What existing frameworks or packages are available? How will the project be deployed? Who are the users? Is the project worth the effort? How long will it take? How will the project be supported? These are the high-level questions that will shape the direction and scope of the project. 

Use the [EI Data Driven Product - Product Definition](https://docs.google.com/document/d/18tqPRzVUzHOwHV_MJVtf4ewP7XH1Kr0QZQ_6B8St56k/edit?usp=sharing) to get started.

Next, think about the general approach. What will the architecture of the project be? Which specific packages will be used? Should you set up a virtual environment? What will the user interface look like? How will the backend be managed? What testing approach will be used? What conventions will be used for file and folder naming, code style, etc.?

Draft a Tool Specifications document if warranted (i.e., for large, billable projects). See the draft Tool Specifications outline in the EI Data Driven Product - Product Definition.

You should have a clear plan in writing before starting with the first line of code.

### Example Project Plan

Here's an example project plan for this project:

#### Goal & Objectives

##### Goal

Provide a single source for documenting and sharing EI's approach to data product development and best practices for current and future staff.

##### Objectives

1. Compile all existing resources for data product development and create single platform for accumulating new resources.
2. Present information in user-friendly format that balances instructional content with requirements for illustrating in-line code.
3. Require staff focused on product development to work with technologies that will be used in deploying EI data products.

#### Users

##### Primary
EI's metrics staff and other technical staff

##### Secondary
Non-technical staff working with metrics staff to develop a data product

#### Technologies:

* MkDocs - static site generator that requires Markdown
* Markdown - markup text language
* Typora - Markdown editor
* VS Code - IDE
* Github - Repository
* Github Project Pages - Deployment (gh-pages branch); see [MkDocs deployment documentation](https://www.mkdocs.org/user-guide/deploying-your-docs/).
* Screen2Gif - a screen recording app that saves outputs as gifs (for video instruction)

#### Architecture

The MkDocs package will create the basic architecture when [creating the project](https://www.mkdocs.org/#getting-started). After creating the project, a mkdocs.yml file will be created. A docs folder will also be created with an index.md file within it. The index.md file manages the site outline; the mkdocs.yml file manages the settings. I'll add a README.md file in the root folder that will show up on the Github repo page. Files and folders can be created within the docs folder to create the project pages. Here's the file structure proposed within the root folder; the folder structure will mirror the site outline:

* mkdocs.yml
* README.md
* docs/
    * index.md
    * project-planning/
        * file-organization-and-naming.md
        * project-planning.md
        * specifications-outline.md
        * skills-and-training.md
    * git/
        * installing-git.md
        * initializing-git.md
        * using-git.md
    * development/
        * virtual-environments.md
        * IDEs.md
        * data-science-workflow.md
    * deployment/
        * deployment-overview.md
        * jupyter.md
        * heroku.md
        * linux.md
        * aws.md
        * docker.md
    * data-management/
        * database-overview.md
    * data-science/
        * workflow-overview.md
        * data-exploration.md
        * data-analysis.md
        * data-visualization.md (e.g., inline exploratory)
    * spatial-analysis/
        * earth-observation.md
        * google-earth-engine.md
        * gdal.md
        * arcpy.md
        * land-use-land-cover.md
    * dashboards/
    * visualization/ (e.g. report quality)
    * packages/
        * dash
        * pandas
        * seaborn
        * folium
        * sqlite
        * rasterio
    * consulting/
        * program-requirements.md
    * conservation-design/

#### Approach

As you can see, lots to be done! The folder order is roughly the prioritization for these pages. Thus, my approach will be to work through these pages in roughly this order. One objective for this project is to provide a place to store new information as it becomes available, so I'll create the above folders initially as markdown files of the same name within a 'tbd' folder where I can store links and other references as I come across them. I've also built some of the pages above in other formats (Evernote, Google Docs, Jupyter Notebooks, etc.) so I can now pull everything together into one place.

#### Other Considerations

Keep track of concerns and other considerations as you go and revisit the specifications periodically to ensure the best approach has been taken.

* How will this tech stack allow for illustrating using code? Can code be run within the deployment environment, or will static code blocks and outputs be needed? How often should links to a Jupyter Notebook, for example, be used as opposed to illustrating static code?
* Is there a good way to surface content for non-technical staff that are interested in these services or are asked by the metrics staff to, for example, complete a product definition? Or should the users be limited to technical staff only?

