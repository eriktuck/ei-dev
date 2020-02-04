# Dashboards

Dashboards promise to distill meaning from complexity and facilitate at-a-glance performance monitoring. However, if poorly designed, they can result in an overwhelming array of incoherent data visualizations that confuse more than illuminate. This page will help you [design a dashboard](#designing-a-dashboard) and [select a platform](#selecting-a-platform) to implement it.

## Designing a dashboard

If you haven't already, review the [introduction to data visualization](data-visualization.md); it will help you through the process of designing the components of the dashboard. Here are some additional considerations specific to design of a dashboard.

#### Consider audience and purpose

As with any data visualization, you'll want to start by considering the purpose of the dashboard and who will be using it. You should list and prioritize the questions you'd like your dashboard to address before you start designing.

Dashboards are commonly categorized as either operational, strategic, or analytical. Operational dashboards focus on monitoring the status of key components of the system. Strategic dashboards allow for quick review of performance indicators. Analytical dashboards are intended to facilitate understanding of the system. Because the purpose of these dashboards are different, the design will also likely differ.

#### Be concise and purposeful

There is a real temptation to throw everything you can on a dashboard. Resist. If you can eliminate something, do it. If you can combine two things without cluttering or confusing, do it. Don't add animations or interactions simply because you can. 

Remember that your purpose is to help a decision maker answer a question quickly. Always weigh the value of an additional feature against the value for the decision maker.

#### Keep it to one view

In general, aim to keep your dashboard to a single screen. At the very least, you want to have one view that provides all of the highest priority information. If you must include more content through scrolling or additional pages, group content thematically so that there is no need to flip back and forth or scroll up and down. This might even mean repeating key information across multiple views.

If you want your dashboard to work on mobile devices, you'll need a responsive design. Keep in mind that this will almost certainly mean a different flow of information. It might not be possible to have all information on a single screen. Treat the responsive layout as its own layout.

#### Use interactions strategically

Interactions allow the user to drill down into data, manipulate parameters, or personalize the dashboard. These can be very helpful for progressively revealing information while maintaining the layout and size of your dashboard. 

However, the purpose of your dashboard is to surface information, not hide it behind a bunch of widgets. Your dashboard is not a puzzle. Avoid burying important information in interactions. 

You may also want to link to source data to allow the user to quickly access key resources.

#### Consider layout and flow

The user's eye typically follows an 'F' pattern across a screen, glancing from left to right while progressing down the left margin (depending on your region). Thus, place the most important elements in the top left of the screen. The bottom right will have the least important information. Group like content together. Finally, use white space liberally. An overly-dense data display can be disorienting and difficult to read.

Dashboards are often designed with *cards*, which allow for easy alignment and responsive design. Each card represents one element. You might divide your dashboard into a few rows and a handful of columns before deciding which element should go where. One element may take up multiple cells, but no cell should be split between multiple elements.

#### Be consistent with formatting

Make sure numbers are formatted correctly. Use sans-serif fonts. Make labels descriptive. Use well-known abbreviations when possible to reduce clutter (but make sure your audience knows what they mean). Repeat colors across multiple elements to highlight their identities. Keep text horizontal as much as possible. Consistent formatting will help communicate information and make your dashboard more appealing to the eye.

#### Provide context

Metrics should be presented with context. Show change over time, indicate the change since yesterday, illustrate the difference between the number and its target, color low performance red and high performance green. Just make sure your audience can quickly gauge the significance of any number and identify trends when it is important. If two indicators should be evaluated in relation to one another, keep them together.

## Selecting a platform

Dashboards are having a moment right now as startups and Fortune-500 companies alike are increasingly using data in their decision making. Platforms abound. Many of these are designed for marketing analytics and business intelligence use cases, but should have sufficient functionality for our purposes. Here are a few considerations when selecting a platform.

#### Cost

There are free options and there are subscription-based options. Subscriptions are not unreasonable, ranging from $30 to $200 a month and up. The budget options limit features like the amount of data you can use, the number of data sources, the number of dashboards, and the frequency of data updates. Carefully review the pricing tiers and their associated limitations before selecting a paid option.

The existing free options are more than sufficient for our purposes in my opinion, and are the only platforms I've reviewed here. 

#### Sharing and hosting

Consider how you will share the dashboard and where it will be hosted. I would suggest that to be really useful the dashboard should be available on the web. Many cloud-based platforms are available for hosting a web-based dashboard. However, make sure you don't share any personal or sensitive information publicly.

#### Integrations

Where do your data live now, and how are your data generated? You'll want whatever solution you select to integrate with your data source(s). Most solutions support an array of integrations, but if you are committed to using Google Sheets, and the platform doesn't support a Google Sheets integration, it won't work for you.

#### Simplicity and customization

There is a tradeoff between simplicity and customization. The truly easy-to-use platforms do not offer much in the way of customization (which is why they are easy to use). You won't always know what a platform can't do until you try to make it do that thing, which can be very frustrating. Stick to the reviewed platforms below and we can help you understand what can, and can't, be done.

#### Blending and computation

Data blending is the process of taking two data sources and combining them in a single dashboard. You may also want to do some computation on the data. Different platforms have different support for blending and computation. Many platforms require you use SQL (Structured Query Language) to do anything but the most basic data blending. Computations are typically performed in a language similar to what you would use in Excel or Google Sheets (e.g., SUM(A, B)).

If you need to do much computation, and you don't want to build an intermediary step into your data pipeline to do the computation, you will need to select a platform that has robust computation support.

#### Growing with your platform

Finally, consider how your dashboard needs may grow over time. Can you easily extend your chosen platform as you grow? You might need to add dashboards for different teams, handle larger amounts of data, customize views for different users, etc. Most solutions will be capable of growing with your needs, but with subscription-based options that may come at a cost.

### Platforms reviewed

The platforms reviewed below were selected because they include a free tier and have reasonably good functionality as dashboards. Keep scrolling to see examples of each.

* Excel/Google Sheets
* Dash
* Tableau Public
* Google Data Studio
* Power BI
* Fine Report

Others include Plecto, Geckoboard, Chartio, Looker, Klipfolio, Holistics, Sisense, Domo, Qlik, Databox, DataHero - literally so many options. If you're able to pay for a platform, evaluate a number of options against the considerations listed above to find one that best meets your needs and budget.

### Excel/Google Sheets





### Dash

### 



### Tableau Public





### Google Data Studio





### Power BI





### Fine Report

Last and, in my opinion, least is Fine Report. This is actually pretty popular software for developing dashboards, but I find it's aesthetic quite poor. They seem to have spent most of their time developing widgets that would look good in Minority Report but don't actually communicate any meaning.

However, Fine Report does include some data entry and management functionality like custom forms.

Use this if you 