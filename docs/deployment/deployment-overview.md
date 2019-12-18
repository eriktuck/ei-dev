# Getting Tools to Users

Tools are only useful if they are used.

Getting tools in the hands of your users, while ensuring a good user experience with tech support and the development of new features, is at least half the battle.

A number of options exist for deploying tools. The deployment environment should be decided early on in tool development.

Here's a short list of options (and important supporting technologies) for deploying tools. Feel free to get creative and invent your own solutions as well.

* **Data Visualization & Dashboards**
     * Plot.ly
     * Bokeh
     * Cufflinks
     * Folium (maps for dashboards)
     * Dash
     * Tableau
     * Ipywidgets, ipyleaflet, …
     * Microsoft PowerApps
     * PowerBI
     * Jupyter Notebook + Binder
     * Google Colab


* **Web Deployment**
     * Flask
     * Django
     * Streamlit
     * [Heroku](heroku.md)
     * GitHub Pages
     * Linode
     * Digital Ocean
     * Google Apps


* **Desktop Deployment**
     * Tkinter
     * Cron Tab
     * PyOxidizer
     * Excel + VBA
     * Command Line Tools


* **[Databases](../data-management/database-overview.md) & Back-ends**
     * SQLLite
     * PostgreSQL
     * MySQL
     * Google Sheets
     * AWS S3


* **Environments**
     * Docker


* **Geographic Data & Mapping**
     * Pyproj (for projection conversion)
     * Google Earth Engine
     * Custom script tools using ESRI ArcGIS API (arcpy)
     * ESRI ArcGIS Story Map
     * ESRI ArcGIS Apps

## Comparing the options

#### Dash vs. Bokeh

Dash and Bokeh provide similar capabilities for developing dashboards. The first step will be selecting which package to learn. [Dash ](https://dash.plot.ly/?_ga=2.113229507.655212128.1562163152-411957839.1562163152)is maintained by Plot.ly, whereas Bokeh is maintained by Anaconda. Based on a little bit of [research](https://blog.sicara.com/bokeh-dash-best-dashboard-framework-python-shiny-alternative-c5b576375f7f), it appears that Dash is a better first choice for building Dashboards. It has slightly less functionality but is 100% python and well documented with rapid development of new features. It offers a free and paid version. Dash also offers a deployment server (paid) and uses Flask as a backend, which is another advantage in deployment. Dash recommends Heroku for deployment if not using the Dash server.

#### Ipywidgets

Plotly plays well with ipywidgets in Notebooks, which provides a nice place to do data exploration or play with dashboard designs. You can pass chart design elements as interactions to test different marker sizes, opacities, background colors, map projections, etc. without interacting with code. See [Jon Mease’s talk at PyData 2018](https://www.youtube.com/watch?v=cuTMbGjN2GQ&t=1440s).

#### Folium

Folium is a python wrapper for Leaflet, a popular JavaScript web map package. It creates HTML maps which conveniently run in Notebooks natively (ipyleaflet is an alternative if using widget integration in a map, but Folium has good interactions without the need for widgets). When combined with Dash, it allows for maps integrated into dashboards.

#### Heroku

[Heroku ](https://devcenter.heroku.com/)is a hosting service for python web apps. It is a fairly [easy to use](https://devcenter.heroku.com/articles/getting-started-with-python) service with a free option and paid options for higher traffic. We’ll use Heroku for our web apps for now. Flask also provides a list of [web deployment options]( https://flask.palletsprojects.com/en/1.1.x/deploying/ ). [OpenShift](https://www.openshift.com/products/pricing/?extIdCarryOver=true&sc_cid=701f2000001Css5AAC) allows one free app.

## The Magic Mix

Heroku, Postgres, Flask, Dash, Plot.ly, Git

The above technologies are integrated in the Heroku platform for web app deployment. Mastering these technologies will provide an important platform for quickly building web apps in the future.