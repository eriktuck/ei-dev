This document serves as a quick notepad for storing helpful information. 

#### Customs Used

To identify a user-provided value, wrap it in <> within a code block:	`git commit <updated-file>`

Highlight package names in code blocks: `rasterio`



#### Markdown Reference

Here is a [handy reference]( https://support.typora.io/Markdown-Reference) for writing in markdown, provided by Typora.

#### Converting Notebooks

To add a notebook to the site, us `nbconvert` to convert the notebook to markdown. Save the output with the desired filename and into a new directory. The new directory will include the markdown file and assets within a *_files directory. Cut and paste the new directory into the ei-dev docs/ folder in the desired location.

1. Open the Anaconda Prompt window within the notebook's root directory

2. Run the following command:

   `nbconvert <notebook.ipynb> --to markdown --out_dir='<new dirname>' --output <desired-filename>`

3. Cut and paste the newly created directory into the ei-dev docs/ folder in the desired location.

Syntax highlighting is done using the `codehilite` package (part of standard markdown library) and `Pygments` (part of standard Python library). However, `codehilite`  must be enabled in the `mkdocs.yml` file. See [documentation]( https://squidfunk.github.io/mkdocs-material/getting-started/#extensions) for more.

Changes made to the notebook will not be reflected in the markdown files, and you'll have to overwrite the folder you created the first time. If you change the name of the outputs, update the `mkdocs.yml` folder.

#### Linking Documents

To link to another page, simply provide the relative path to the page as a markdown file. You can link to a section if you add a '#' after the filename. Sections should be all lower-case and '-' delimitted. See also [documentation]( https://mkdocs.readthedocs.io/en/0.11.1/user-guide/writing-your-docs/#linking-documents).

`[here's a link](foldername/filename.md#header-name)`

#### Images and Gifs

You can either link to images and gifs online or save locally. If saving locally, store in `assets/` within the section directory (e.g., `consulting-process/`). The alternative text (within the square brackets) is read by accessibility services. See also [documentation]( https://mkdocs.readthedocs.io/en/0.11.1/user-guide/writing-your-docs/#images-and-media)

`![here's an image](assets/gif.gif)`

#### Youtube Videos

To include Youtube videos, use the iframe provided under the 'Share' option. Any HTML pasted directly within a markdown file will be rendered in the site.

#### Extensions

Markdown and Mkdcos both offer extensions for rendering markdown. Note that the extensions may need to be configured for Typora as well as Mkdocs for consistency between editing and serving. Material has a few extensions as well. Check these out in their project documentation pages to see if the functionality would be helpful.

#### Interactive Visualizations

Maps, dashboards, and other interactive visualizations should be passed in using the iframe. See Plotly guidance [here]( https://plot.ly/python/embedding-plotly-graphs-in-HTML).