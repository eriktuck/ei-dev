# Intro to Data Visualization

Data visualization is the art of telling a story, with data, visually. 

A good storyteller will consider their audience, provide context, focus on what's important, and deliver the payoff in the end. As will a great visual data storyteller. This tutorial will help you on your path to being a great visual data storyteller, and provide suggested technologies to make your ideas come to life.

> There are thousands of resources available to you to improve your visualization skills, including [online classes](https://www.tableau.com/learn), YouTube videos, web articles, [blogs](https://www.tableau.com/learn/articles/best-data-visualization-blogs), [books](https://www.tableau.com/learn/articles/books-about-data-visualization), [competitions](https://www.makeovermonday.co.uk/data/), etc. You can even get an advanced degree in data visualization if you're so inclined. What follows is simply a collection of the best concepts and advice I've encountered. I encourage you to explore other sources, experiment, and even throw this right out if it doesn't speak to you.
>

## The process

*Most stories - at least the ones worth listening to - begin with a question. How did you get that scar? Great story actually…*

Once you've identified a data visualization need, consider the people who make up your audience: for which questions will they need answers? **List all the questions** you can think of and highlight the most important. You might find it useful to group similar questions.

Next, **inventory the data sources** at your disposal. You will, after all, need data to create a data visualization. As is typically true of working with data, you may spend a disproportionate amount of your time collecting and cleaning the data you want to use. If the data you want is not attainable, you'll need to reframe your approach.

Before you start cleaning data and creating visuals, *stop*. Step away from the computer. Find a sunny patio or a dimly lit coffee shop. Get out a piece of paper (I prefer the gridded engineering graph paper). Grab your pencils (colored pencils if you like - although I recommend designing first in black and white before judiciously adding color). **Sketch**. 

Pick a question from your list that is the most interesting to you. Given your understanding of the data available, how would you answer that question with a data visual? Which [type of chart](https://public.tableau.com/en-us/gallery/visual-vocabulary) might work best? Sketch a few alternatives to see how they compare. Pick another question and repeat.

Sketching first will save you time by helping you explore the story you want to tell and how you want to tell it. You can more quickly prototype ideas on paper than in any visualization software. You also won't feel hamstrung by the limitations of the software, or your limitations in working with the software. Skipping this step is how we end up with so many of the same boring bar charts in Excel's standard color scheme.

Now is a good time to **share your sketches** with members of your intended audience to get their reactions. This is also a good time to co-create visuals with others. You can work together to pick the most important questions to answer and update the sketches to more intuitively answer them. 

With your draft visuals in hand, you should now have a clearer understanding of what [type of visualization](#types-of-visualizations) to develop and which visualization platform to use. **Prepare and explore your data** to see if you can discover new insights. Quickly **test your sketches**. Do the data work with (or against) your sketch? If your visuals will be dynamic (as with a dashboard, for example) test out different slices of the data or create random data to test edge cases.

With many rounds of iteration, you will **develop your visualizations**. Next, **strip away distraction** and focus on the key message. This is where you make sure you deliver that payoff - the information your audience needs as succinctly, intuitively, and insightfully as possible. See the [techniques](#techniques) and [approaches](#how-to-tell-a-story) sections for suggestions.

Finally, **deliver** your visualization. To borrow from Paul Valery, *"a work of art is never finished, merely abandoned"*.

#### The process, succinctly

1. List the key questions of your audience
2. Inventory data sources
3. Sketch potential visuals
4. Share your sketches with members of your audience
5. Prepare and explore the data
6. Test your sketches
7. Develop your visualizations through iteration
8. Strip away distraction
9. Deliver

## Types of visualizations

There's no official taxonomy of data visualizations, but here's a quick overview of a few important visualization types in my opinion:

* **Chart**: your standard static data visualization. The static chart's primary habitat is in reports and other static media. See also graphs. 
* **Infographic**: a thematic collection of multiple static visualizations, often presented as a poster or banner.
* **Dashboard**: a thematic collection of one or more dynamic visualizations, updated at some regular interval. 
* **Data Story**: a presentation of data that marries narrative with data visualization. See some great examples under Inspiration.

Any of the above can support interactivity, animations, and/or maps. A static map would be considered a chart (just ask any sailor). By static I mean the underlying data do not change; data change with dynamic visualizations - which provides a unique challenge for visualizations.

## Visualization tools

There are dozens (hundreds?) of visualization tools available to you. If you don't already have a preferred solution, here are a few suggestions. This list focuses on tools with a free tier. Of course, there's always Excel or Google Sheets - nothing wrong with these options - but I'm assuming you've already been introduced.

* **Tableau**: these folks are visualization nerds and incorporate by default the latest in visualization science in your visuals - which makes you look smart. [Tableau Public](https://public.tableau.com/en-us/s/) is free but, you guessed it, makes your visualization public (so don't choose this solution if you're working with sensitive data, or get a paid account).
* **Power BI**: [Power BI](https://powerbi.microsoft.com/en-us/downloads/) is a free desktop tool with an online service to allow you to publish your visuals and create dashboards. If your team is already using MS Office, it’s a good choice for its easy integration with the Microsoft ecosystem and ability to share visuals within your organization.
* **Google Charts & Data Studio**: this is Google's answer to Microsoft's Power BI. Slick and intuitive, you can very quickly learn to create [visuals](https://developers.google.com/chart) and [dashboards](https://support.google.com/datastudio/answer/6283323?hl=en) in an afternoon that look like they took a week.
* **Plotly & Dash**: Plotly's open-source [Chart Studio](https://chart-studio.plot.ly/create/) allows you to upload data and quickly create interactive web-ready charts from their standard library. However, this solution really sings if you're able to use the [Dash](https://dash.plot.ly/?_ga=2.77431061.1642172462.1580333706-1252667748.1577723508) API with Python. Use this solution to develop and deploy analytic web apps, like interactive dashboards.
* **Infogram**: an option for creating infographics, [Infogram](https://infogram.com/) allows up to 10 projects at the free tier. See also [Visme](https://www.visme.co/), [Adobe Illustrator](https://www.adobe.com/products/illustrator), and [Inkscape](https://inkscape.org/).
* **Matplotlib & Seaborn**: for Python developers, these are your go-to libraries for data visualization.
- **ArcGIS StoryMaps**: StoryMaps include straightforward interactive maps and the eponymous [Story Map](https://storymaps.arcgis.com/stories/cea22a609a1d4cccb8d54c650b595bc4), a platform for creating data stories that typically (but don't have to!) include maps and other spatial components.

## More tips on visualization

If you're wondering which visualization to use for a given task, start with this [Visualization Vocabulary](https://public.tableau.com/en-us/gallery/visual-vocabulary).

### Techniques

Try out some of these techniques to improve your visualizations.

- Highlighting
- Transition Guidance (layering the story, point by point)
- Ordering
- Messaging
- Clarity
- Reduce but don't avoid complexity
- Interaction

### How to tell a story

These are approaches to telling your data story. 

- Change Over Time
- Drill Down
- Zoom Out
- Contrast
- Intersections
- Factors
- Outliers
- Communicating Uncertainty

## Inspiration

*How did I create an entire post about data visualization without a single data visualization?* Check these out:

#### Galleries

- [Tableau Public Gallery](https://public.tableau.com/en-us/gallery/?tab=viz-of-the-day&type=viz-of-the-day)
- [Makeover Monday Gallery](https://www.makeovermonday.co.uk/gallery/)
- [Dash App Gallery](https://dash-gallery.plotly.host/Portal/)

#### Philosophies

- [Data Humanism](http://giorgialupi.com/data-humanism-my-manifesto-for-a-new-data-wold)

#### Some great examples

- [The Avalanche at Snow Falls  (NYT)](http://www.nytimes.com/projects/2012/snow-fall/index.html#/?part=tunnel-creek)
- [What's really warming the world (Bloomberg)](https://www.bloomberg.com/graphics/2015-whats-warming-the-world/)
- [Bussed Out (The Guardian)](https://www.theguardian.com/us-news/ng-interactive/2017/dec/20/bussed-out-america-moves-homeless-people-country-study)
