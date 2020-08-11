# Google API

Google provides an API for interacting with Google Drive and its suite of office products (Sheets, Docs, Slides, etc.). An API is an 'application programming interface'. Simply put, it's the interpreter between your program and Google's products. If you want to write into a Google Doc, you can use the API to do it.

Google Sheets is actually a quite powerful backend data store for web-based applications. If you're deploying to [Heroku](heroku.md) and don't have the need to configure a Postgres database, Google Sheets can get the job done quite well. That said, it's not free of issues, so beware before building anything mission critical.

You can create up to 12 projects on Google Drive using the API for free.

### Setting up access

[This tutorial](https://www.twilio.com/blog/2017/02/an-easy-way-to-read-and-write-to-a-google-spreadsheet-in-python.html) describes how to set up a project through Google's API Console and get the credentials you'll need to access it. Basically

1. Go to the [Google API Console](https://console.developers.google.com/)
2. Create a new project
3. Click Enable API. Search for and enable the Google Drive API.
4. Search for and enable the Google Sheets API.
5. Create credentials for a Web Server to access Application Data.
6. Name the service account and grant it a Project Role of Editor
7. Download the JSON file
8. Copy the JSON file into your project directory under a subdirectory called `secrets/`
9. In the spreadsheet on Google Drive, click the Share button and paste the client email from the JSON file into the People field to give it edit rights. Hit Send.

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/original_images/google-developer-console.gif)

Credit: Greg Baugues, twilio.com

### Interacting with the spreadsheet

Now you can access the spreadsheet from your script. You'll need to install `pygsheets` and `pandas`; `conda install` the required packages. Follow the instructions above to save your credentials into a `secrets/` directory within the project. Learn more about how to handle credentials with Heroku [here](heroku.md) (step 23).

#### Authorizing access

```python
import pygsheets
def auth_gspread():
    """Authorize Google to access the Utilization Project
    :returns client
    """
    # creds for local development
    try:
        client = pygsheets.authorize(
            service_file='secrets/gs_credentials.json'
            )
    # creds for heroku deployment
    except:
        client = pygsheets.authorize(
            service_account_env_var='GOOGLE_SHEETS_CREDS_JSON'
        )
        
    return client
```

Refer to the `pygsheets` [docs](https://pygsheets.readthedocs.io/en/stable/reference.html) for help reading and writing to spreadsheets; or this [helpful guide](https://medium.com/game-of-data/play-with-google-spreadsheets-with-python-301dd4ee36eb) from Towards Data Science.

#### Reading data to a data frame

Use this code to read a spreadsheet as a pandas data frame use the function above to get the `client` variable). Note that a spreadsheet (`sh`) is synonymous with an Excel workbook, and a worksheet (`wks`) is still a worksheet.

```python
import pandas as pd

def load_data(client, spreadsheet, sheet_title, tidy=False):
    """Load data (must be in tidy format) from sheet.
    Empty rows are dropped.
    :param client: client object for accessing google
    :param spreadsheet: name of the spreadsheet
    :param sheet_title: worksheet name
    :param tidy: if data is well-formatted, set to True
    :returns pandas dataframe
    """
    # load data from google sheet
    sh = client.open(spreadsheet)
    wks = sh.worksheet_by_title(sheet_title)
    if tidy:
        df = wks.get_as_df(empty_values=None)
    else:
        data = wks.get_all_records(empty_value=None)  # get_as_df can't handle empty columns
    	df = pd.DataFrame(data)
    df.dropna(axis=0, how='all', inplace=True)
    
    return df
```

#### Saving a data frame

To save a csv in an existing worksheet (note this will overwrite any data in the worksheet):

```python
def save_data(client, spreadsheet, sheet_title, df)
	"""Save data to a worksheet, overwriting existing data
	:param client: client object for accessing google
    :param spreadsheet: name of the spreadsheet
    :param sheet_title: worksheet name
    :param df: dataframe to save
    :returns none
    """
    sh = client.open(spreadsheet)
    wks = sh.worksheet_by_title(sheet_title)
    wks.set_dataframe(df, 'A1', fit=True)
```

