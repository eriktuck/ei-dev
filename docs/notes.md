# Tips & Tricks

#### Customs Used

To identify a user-provided value, wrap it in `<>` within a code block:	`git commit <updated-file>`

Highlight package names, files, code, and cli commands in code blocks: `rasterio`

#### Markdown Reference

Here is a [handy reference]( https://support.typora.io/Markdown-Reference) for writing in markdown, provided by Typora.

#### Converting Notebooks

To add a notebook to the site, us `nbconvert` to convert the notebook to markdown. Save the output with the desired filename and into a new directory. The new directory will include the markdown file and assets within a *_files directory. Cut and paste the new directory into the ei-dev docs/ folder in the desired location.

1. Open the Anaconda Prompt window within the notebook's root directory

2. Run the following command (a new directory will be created if needed):

   `jupyter nbconvert <notebook.ipynb> --to markdown --output-dir='<rel path to new dirname>' --output <desired-filename>`

>For example `jupyter nbconvert my_notebook.ipynb --to markdown --output-dir='../md-notebooks' --output my_markdown`

3. Cut and paste the newly created directory into the ei-dev docs/ folder in the desired location.

Syntax highlighting is done using the `codehilite` package (part of standard markdown library) and `Pygments` (part of standard Python library). However, `codehilite`  must be enabled in the `mkdocs.yml` file. See [documentation]( https://squidfunk.github.io/mkdocs-material/getting-started/#extensions) for more.

Changes made to the notebook will not be reflected in the markdown files, and you'll have to overwrite the folder you created the first time. If you change the name of the outputs, update the `mkdocs.yml` folder.

#### Linking To and Within Documents 

To link to another page, simply provide the relative path to the page as a markdown file. You can link to a section if you add a '#' after the filename. Sections should be all lower-case with '-' in place of spaces (make sure all headers are unique as well). Ignore any punctuation in the header. If in the same page, omit the page name. See also [documentation]( https://mkdocs.readthedocs.io/en/0.11.1/user-guide/writing-your-docs/#linking-documents).

`[here's a link to another document](foldername/filename.md#header-name)`

`[here's a link to a head in this document](#header-name)`

#### Images and Gifs

You can either link to images and gifs online or save locally. If saving locally, store in `assets/` within the section directory (e.g., `consulting-process/`). The alternative text (within the square brackets) is read by accessibility services. See also [documentation]( https://mkdocs.readthedocs.io/en/0.11.1/user-guide/writing-your-docs/#images-and-media). Use web links whenever possible.

`![here's an image](assets/gif.gif)`

#### YouTube Videos

To include Youtube videos, use the iframe provided under the 'Share' option. Any HTML pasted directly within a markdown file will be rendered in the site.

#### Forms

Google forms (and probably many other web forms) can be embedded as HTML within an iframe.

#### Extensions

Markdown and Mkdocs both offer extensions for rendering markdown. Note that the extensions may need to be configured for Typora as well as Mkdocs for consistency between editing and serving. Material has a few extensions as well. Check these out in their project documentation pages to see if the functionality would be helpful. The site currently uses the Material theme extension.

Some interesting extensions:

##### Admonition

Provides callout style boxes, including collapsible boxes, for example:

!!! warning
    Don't do this really bad thing!
!!! info "For your information..."
    ...I changed the title of this note.

See [documentation](https://squidfunk.github.io/mkdocs-material/extensions/admonition/) for more.

#### Interactive Visualizations

Maps, dashboards, and other interactive visualizations should be passed in using the iframe. See Plotly guidance [here]( https://plot.ly/python/embedding-plotly-graphs-in-HTML).

#### HTML Tags

Any HTML written in the document will get passed as pure HTML when served. If the standard markdown isn't sufficient, use pure HTML. For example, to get the credit for the comic on the home page to be right adjusted, I typed this line (don't include block quotes if you want the actual text rendered rather than the code) :

```html
<div style = "text-align: right">  Credit: Randall Munroe (xkcd.com/2054) </div>
```

A few tags to consider:

##### Details 

This tag allows for drop-down type hiding of content.

```html
<detail>
<summary>The text when collapsed goes here</summary>
<p>The text that gets hidden goes here</p>
</detail>
```

It's best to work within the source code mode (`ctrl+/`), as Markdown treats this strangely if trying to pass markdown between the detail tags.

<details>
<summary>Click me!</summary>
<p>This would be the first paragraph of content</p>
<p> Here's some more content with a <a href='https://enviroincentives.com'>link</a></p>
</details>

Note that the `details` tag is styled by the extension [admonition](#admonition) as a call out box. I couldn't prevent this behavior by removing the admonition extension from my yaml file when using the Material Theme. [Adding custom CSS](#css) or switching to a different theme may help override this behavior. 

##### PyMdown

A ton of functionality in this [package of extensions](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/), including highlighting text, inline comments and editing, math equations, and task lists (though not interactive task lists).


#### JavaScript

You can add custom JavaScript to MkDocs provided you're ok with the script running on every page. 

In the `mkdocs.yml` file, add a line that points to the location of the JavaScript file relative to the docs/ folder:

```yaml
extra_javascript:
  - javascripts/scripts.js
```

The script will automatically be included on every page of the document, no need to add a script tag.

Here's an example that I've implemented on the site. I want all external links to open in a new window, and internal links to open in the same window. I could express each link in html, but it would be easier to update all links written in standard markdown (which doesn't have this functionality out-of-the-box). 

Here's the content of the JavaScript file:

```javascript
var links = document.links;

for (var i = 0, linksLength = links.length; i < linksLength; i++) {
  if (links[i].hostname != window.location.hostname) {
      links[i].target = '_blank';
  } 
}
```

This script gets a list of all links in the document, checks whether the hostname is the same as the hostname of the current window (i.e., the hostname of my website), and then updates the target property of each link to `"_blank"` whenever the link is external.

 Note you can also add JavaScript (and CSS) by defining a [custom theme](https://www.mkdocs.org/user-guide/custom-themes/).

#### CSS

See also [JavaScript](#javascript) above, but basically just include in the `mkdocs.yml` file the following:

```yaml
extra_css:
  - stylesheets/<my_css.css>
  - stylesheets/<more_css.css>
```
