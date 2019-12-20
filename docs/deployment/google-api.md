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

Now you can access the spreadsheet from your script. You'll need both the `gspread` and `oauth2client` package. *Note that oauth2client is deprecated, see `google-auth` or `oauthlib`, `gspread` may be superseded by `pygsheets`*.

`conda install` the required packages.

Use the code below to get access to the spreadsheet. 

```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials

scope = ['https://spreadsheets.google.com/feeds',
         'https://www.googleapis.com/auth/drive']

creds = ServiceAccountCredentials.from_json_keyfile_name(
	'<path to JSON credentials>', scope
)

client = gspread.authorize(creds)
```

Refer to the 'gspread` [docs](https://gspread.readthedocs.io/en/latest/) for help reading and writing to spreadsheets.

#### Basic gspread commands

Use this code to read a spreadsheet as a pandas data frame (include code from above to authenticate). Note that a spreadsheet is synonymous with an Excel workbook, and a worksheets is still a worksheet.

```python
import pandas as pd

wks = client.open('<spreadsheet name>').worksheet('<worksheet name>')
data = hours_wks.get_all_values()
headers = data.pop(0)
df = pd.DataFrame(data, columns=headers)
```

To save a csv in an existing workbook (note this will overwrite the entire workbook):

```python
out_file = client.open('<spreadsheet name>')'.id
data = ('<data.csv>')
client.import_csv(out_file, data)
```

