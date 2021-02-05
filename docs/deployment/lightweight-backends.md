# Lightweight Backends

For some applications, you don't need or want to go through the extra effort of setting up a database backend. You have a few options for hosting data for web-based applications and similar tools. This topic covers a few of those options.

## Google Sheets (no API required)

Google Sheets can be downloaded automatically as CSV files using a URL that you can construct from the sheet’s key. Take this sheet, for example:
`https://docs.google.com/spreadsheets/d/1qrtaGu5dwljVbHJeR4RIMWtdZKOauJfJ6nODlPwMRno/edit?usp=sharing`

The “key” is the long string appearing after “/d/”:
`1qrtaGu5dwljVbHJeR4RIMWtdZKOauJfJ6nODlPwMRno`

You can create a CSV download link for that sheet, by plugging the “key” into this URL structure:
`https://docs.google.com/spreadsheets/d/{key}/gviz/tq?tqx=out:csv`

!!!Tip
    You can also get a download link by downloading the file as csv from Google Sheets (FIle> Download> Comma separated values), navigating to your downloads folder (Ctrl+J), right clicking the link and selecting 'Copy Link Address'.

Any application with that link will download a csv file directly.

If you have multiple sheets, specify the sheet by appending `&sheet=<sheet>`.

You can, for example, pass data from one Google Sheet to another by using the IMPORTDATA() function. In the cell you want to import these data, type:

```
=importdata("https://docs.google.com/spreadsheets/d/<key>/gviz/tq?tqx=out:csv&sheet=<sheet>")
```

!!!Info
    [IMPORTDATA refreshes every hour](https://support.google.com/docs/answer/58515?hl=en), so your spreadsheet may not have up-to-date information if a change was made recently.

## Google Sheets (with pygsheets)

Google Sheets can be a great lightweight cloud database for you application. Check out the [Google API](google-api.md) tutorial for an in depth discussion of how to set this up.

## GitHub

GitHub makes available any csvs committed to a repo through a 'raw' link. Navigate to the csv file and select the 'raw' link.

![github_raw](assets/github_raw.png)

Provide that url to your application.

This is a good option if you have a very stable database (i.e., doesn't require changes) or you will be pushing updates to the files regularly. Be careful not to get your test environment and development environment confused with this option. 