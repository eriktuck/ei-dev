# Google Sheets Application

Imagine you want to leverage the familiarity and ease-of-use of Google Sheets for your user interface but you also want to develop a user-friendly application-like experience for users. Google Sheets can serve as a surprisingly powerful platform for developing your solution with a few simple tricks.

This tutorial will introduce you to the concepts required to extend Google Sheets to an efficient full-stack solution for your application. I am not going to use Macros, AppSheet, or Add-ons, although you may find those useful. This tutorial instead uses out-of-the-box functionality of Google Sheets. I will supply one formula for the Script editor, but it is not important to understand how it works.

The benefits of using Google Sheets is that you can much more quickly build a fairly robust solution than on another platform. It is also likely familiar to user, which allows you to bring them into the development and testing process. Finally, it has a ton of out-of-the box functionality that you can leverage for your project. Google Sheets can be right for prototyping solutions and often can be the first and last stop for developing a lightweight application.

??? note "Why not Excel?"
	Excel is behind Google Sheets in two key areas: array formulas and data sharing. Array formulas allow for data aggregation and import without knowing the structure of the data. Ever copy data from one sheet to another by just dragging a `=Sheet1!A1` formula down a column? You need an array formula. Data sharing between Excel sheets can be accomplished through external links or Power Query, however I find those break more than they work once a file is shared.

## Spreadsheet Maturity Model

Before beginning, I want to share a model of maturity for spreadsheets I first heard in this [GSuite presentation](https://www.youtube.com/watch?v=vnm6ViI06MM&t=1561s).  This is a very useful model for thinking about how to advance your Google Sheet project from concept to application. 

1. **Infancy** - "Capture the Idea" - just trying to figure out what pieces of data and organization of data makes the most sense.
2. **Toddler** - "Validate the Idea" - share the spreadsheet with others to collaborate, improve, and capture more use cases.
3. **Grade School** - "Formalize the idea" - apply formatting, validation, and freeze rows. Generally make it easier to use and supply affordances (hints to users). 
4. **Middle School** - "Build the permissions" - protect ranges, set up permissions, and use private sheets (sharing data through `IMPORTRANGE`). Consider access and privileges.
5. **High School** - "Build the process" - build nuance to the data collection process to reflect users workflows. Set up forms instead of editing the sheet directly.
6. **College Student** - "Build the logic" - automate some processes and improve communications throughout the workflow. (GSuite recommends AppSheet, however the pricing is [...confusing](https://community.appsheet.com/t/appsheet-cost-for-g-suite-business-users/18810)).

If you are using spreadsheets as your app platform, you'll want to have a "College Student" level of maturity before going to production. Separate the data entry functionality from the data storage and data analysis/visualization. This will greatly improve the user experience of your spreadsheet application.

## Data Design

1. Data Entry
2. Data Storage
3. Data Visualization

## "Tidy Endpoints"

Tidy Endpoints is a conceptual extension of RESTful endpoints, however using tidy data instead of JSON. 

[Tidy data](https://vita.had.co.nz/papers/tidy-data.pdf) is a concept popularized by Hadley Wickham and an incredibly simple yet powerful concept for data management. Tidy data can also be described as 'tall' rather than 'wide'. Tidy data correlates to Codd's Third Normal Form, which can be further refined to 4th normal form for efficient data storage (as used in database applications). I've found spreadsheets are more amenable to tidy data than 4th normal form data, so we'll use the tidy data concept for our architecture.

The power of the Tidy Endpoints concept is that it allows us to format our data in such a way that it can be efficiently and safely shared with other spreadsheets without worrying about the structure of those sheets or the total amount of data shared. (However, note that Google Sheets has limits on how much data can be shared.)

Tidy data meets the following conditions:

1. Each variable forms a column
2. Each observation forms a row
3. Each level of observation forms a table

## Array Formulas

To unlock the power of Google Sheets, you must understand [array formulas](https://support.google.com/docs/answer/6208276?hl=en&ref_topic=9055396). Array formulas return an array, rather than a single value. In many ways, the lack of array formulas was the primary constraint of spreadsheets as compared to higher level programming interfaces. Without array formulas, you are acting as a low-level memory manager for the application, allocating a single piece of information to a single cell. With array formulas, you have the power of many modern programing languages as the memory management function is abstracted away.

The simplest way to return an array in Google Sheets is to use curly braces `{}`.

For example, this would return all of the data in columns A and C in side-by-side columns:

```
={Sheet1!A:A, Sheet1C:C}
```
To return data stacked vertically, rather than side-by-side, use a semi-colon:
```
={Sheet1A1:A10, Sheet1!A20:A30}
```

Alternatively, you can use `ARRAYFORMULA(Sheet1A1:A10, Sheet1!A20:A30)` for the same purpose.

### Array Formula examples

Let's try out some array formulas. Note that you won't need to worry about fixed references (`$`) because you won't be dragging the formula around.

#### Multiply a column by a scalar

`=arrayformula(A2:A7*10)`

#### Multiply an array by a scalar

`=arrayformula(A2:F7*10)`

#### Product table

`=arrayformula(A2:A7*B1:F1)`

## Supporting Formulas

We'll use the following formulas in combination with array formulas to aggregate, format, and share data.

#### Filter

`FILTER` is my go-to formula for concatenating. It improves upon the basic array formula by allowing you to filter what is returned in the array. Most often, I filter null or zero values, for example:

````
=FILTER(Sheet1!A:Z, Sheet1!A:A > 0)
````

will return the array in Sheet1!A:Z where column A is greater than zero. 

To pull data from multiple sheets, we can use:

```
={FILTER(Sheet1!A:Z, Sheet1!A:A > 0); 
FILTER(Sheet2!A2:Z, Sheet2!:A2:A > 0)}
```

Note that sheets 2 and beyond should only pull from row 2 on to avoid duplicating headers in your array. Sheet 1 and Sheet 2 should have the same headers.

##### Filtering the results of an array formula

Sometimes you want to filter the results of an array formula. You can use `INDEX` to get the right column to filter for

```
={FILTER(ARRAYFORMULA(Sheet1!A:Z),
INDEX(ARRAYFORMULA(Sheet1!A:Z),0,3)<>"")}
```

will filter the third column of the returned array that are blank. This is useful when the array formula is manipulating the structure of the data, and you do not have access to that column (common with `UNPIVOT`, see below). However, note that this is now calling the array formula twice! This can be very expensive. Use a helper table instead. If you're importing using `IMPORTRANGE`, use a `QUERY`.

##### Filter based on a list

```
=FILTER(A:C, COUNTIF(E:E, A:A))
```

where `E` is the list to check and `A` is the column to check against `E`.

#### Query

`QUERY` is a `SQL`-like syntax for querying arrays. If you are familiar with `SQL` you will want to explore this function. Why isn't it my go-to then? Primarily because it behaves unexpectedly when a column's data types are mixed. 

Imagine your company uses identifiers that contain a mix of letters and numbers, but some (most) are numeric. Query will simply ignore any data that are not of the dominant type in a column (and it won't pay attention to how you have formatted the column!). Thus, you may end up missing values (without warning) in your results.

`QUERY` relies on a SELECT statement passed as a string to return your array. You will not have access to the column headers for the SELECT statement, so you must refer to columns by their column letter (i.e., `A`, `B`). Alternatively (and if you are combining a `QUERY` with `IMPORTRANGE`) you can wrap your range in curly braces and use 'Col1', 'Col2', ... to refer to columns by order. The reason this works is that the range provided is now no longer considered part of the spreadsheet, and so you don't have the column letter identifiers. Note that you cannot use 'Col1', 'Col2', ... without wrapping the range in curly braces.

An example `QUERY` might be

```
=QUERY(Sheet1!A:Z, "SELECT A, sum(B) where A is not null group by A")
```

or

```
=QUERY({Sheet1!A:Z}, SELECT Col1, sum(Col2) where Col1 is not null group by Col1)
```

This would return the sum of all values in column B grouped by column A where A is not null (use `<> ''` if the column contains text rather than numerical data). 

##### Using column headers in SELECT statements

If you need to substitute a column header for the column letter, you can use this slightly complicated formula

```
SUBSTITUTE(ADDRESS(1, MATCH("<Column Name>",Sheet1!A$1:$Z$1,0),4),1,"")
```

in the SELECT statement using concatenation

```
=QUERY(Sheet1!A:Z, "SELECT" & <paste SUBSTITUTE formula here>)
```

##### Indirect

Indirect is another way to return a cell address using a string as input, in essence it converts `"A1"` to `A1` so that Google Sheets treats it like a cell address.

##### Advanced Queries

###### Group by

Query can be used with SQL-like grammar for very powerful calculations like groupby.

```
=query(eventsByStaff!A:O, "select A, B, sum(D), sum(E), sum(F), sum(G), sum(H), sum(I), sum(J), sum(K), sum(L), sum(M), sum(N), sum(O) where A is not null group by A, B", 1)
```

##### Filtering Imported data

```
=QUERY(IMPORTRANGE("url", "Sheet1!A:Z") "SELECT * WHERE Col1 = 'TRUE'")
```

Note the quotes around True, otherwise it will look for a column named TRUE (which it won't find).

#### Unpivot

Unpivot is a custom function written using the Script editor. Unpivoting, or 'melting' data is a common practice in data manipulation to get tidy data. If you have columns that are actually dimensions, you can unpivot the data to get a column with dimensions and a column with corresponding value. I've provided the code below (not my code, [credit](https://stackoverflow.com/questions/24954722/how-do-you-create-a-reverse-pivot-in-google-sheets)). Simply open the Script editor, paste the contents of the code block below, hit save, and the `unpivot` function will be available to you in the sheet.

```
/**
 * Unpivot a pivot table of any size.
 *
 * @param {A1:D30} data The pivot table.
 * @param {1} fixColumns Number of columns, after which pivoted values begin. Default 1.
 * @param {1} fixRows Number of rows (1 or 2), after which pivoted values begin. Default 1.
 * @param {"city"} titlePivot The title of horizontal pivot values. Default "column".
 * @param {"distance"[,...]} titleValue The title of pivot table values. Default "value".
 * @return The unpivoted table
 * @customfunction
 * https://stackoverflow.com/questions/24954722/how-do-you-create-a-reverse-pivot-in-google-sheets
 */
function unpivot(data,fixColumns,fixRows,titlePivot,titleValue) {  
  var fixColumns = fixColumns || 1; // how many columns are fixed
  var fixRows = fixRows || 1; // how many rows are fixed
  var titlePivot = titlePivot || 'column';
  var titleValue = titleValue || 'value';
  var ret=[],i,j,row,uniqueCols=1;

  // we handle only 2 dimension arrays
  if (!Array.isArray(data) || data.length < fixRows || !Array.isArray(data[0]) || data[0].length < fixColumns)
    throw new Error('no data');
  // we handle max 2 fixed rows
  if (fixRows > 2)
    throw new Error('max 2 fixed rows are allowed');

  // fill empty cells in the first row with value set last in previous columns (for 2 fixed rows)
  var tmp = '';
  for (j=0;j<data[0].length;j++)
    if (data[0][j] != '') 
      tmp = data[0][j];
    else
      data[0][j] = tmp;

  // for 2 fixed rows calculate unique column number
  if (fixRows == 2)
  {
    uniqueCols = 0;
    tmp = {};
    for (j=fixColumns;j<data[1].length;j++)
      if (typeof tmp[ data[1][j] ] == 'undefined')
      {
        tmp[ data[1][j] ] = 1;
        uniqueCols++;
      }
  }

  // return first row: fix column titles + pivoted values column title + values column title(s)
  row = [];
    for (j=0;j<fixColumns;j++) row.push(fixRows == 2 ? data[0][j]||data[1][j] : data[0][j]); // for 2 fixed rows we try to find the title in row 1 and row 2
    for (j=3;j<arguments.length;j++) row.push(arguments[j]);
  ret.push(row);

  // processing rows (skipping the fixed columns, then dedicating a new row for each pivoted value)
  for (i=fixRows; i<data.length && data[i].length > 0; i++)
  {
    // skip totally empty or only whitespace containing rows
    if (data[i].join('').replace(/\s+/g,'').length == 0 ) continue;

    // unpivot the row
    row = [];
    for (j=0;j<fixColumns && j<data[i].length;j++)
      row.push(data[i][j]);
    for (j=fixColumns;j<data[i].length;j+=uniqueCols)
      ret.push( 
        row.concat([data[0][j]]) // the first row title value
        .concat(data[i].slice(j,j+uniqueCols)) // pivoted values
      );
  }

  return ret;
}
```

Simply specify the data source, number of fixed columns/rows, and optionally the name of the new dimension column and value column.

```
=UNPIVOT(Sheet1!A:Z, 2, 1, "my dim", "my val")
```

Here the first two columns will be treated as fixed and the first row will be treated as the header. Note that only two header rows are supported. If you need to add more structure to your sheet, consider adding hidden rows/columns around your variable data to pull them closer to the data you need to unpivot.

### Non-array Formulas of note

#### Index Match

Combining the `INDEX` and `MATCH` formula is a powerhouse formula for spreadsheets, allowing you to lookup a value from one column based on a value from another column. If you don't know it, check it out, there are lots of great resources online. You cannot use Index Match in an array formula however. 

### Pivot Tables

Unpivoted data is great for sharing and analysis, but not always optimal for data visualizing. Use Google Sheets built in Pivot tables to return data to its original format if needed.

### Joins

If you're familiar with SQL, you'll likely sorely miss the ability to join data, especially when the join is many-to-many. Here's how you can do it in Google Sheets:

```
=FILTER(
{Sheet1!A:C, 
VLOOKUP(Sheet1!C:C, {Sheet2!A:A, Sheet2!B:E}, {2,3,4,5}, false)}, 
Sheet1!C:C<>"")
```

First note that you will be combining two results side by side (Sheet1!A:C and the result of the VLOOKUP).

That will be filtered where Sheet1 column C is not blank.

The VLOOKUP table is in Sheet2, where column A is the join index for Sheet2 and column C is the join index for Sheet1. Columns B:E is Sheet2 will be joined to Sheet1 wherever the join indices match.

The general form is below:

```
=FILTER(
{<Table1>,
VLOOKUP(<Table1 Join Index>, {<Table2 Join Index>, <Table2 Columns to Join>}, {<2, 3 ... length of Table2 Columns to Join>}, false)},
<Filter condition>)
```

### Formula efficiency

Use helper columns and helper tables to avoid re-calculating the same thing multiple times. This can speed up your spreadsheet immensely.

Don't:

```
={FILTER(IMPORTRANGE("url", "SheetName!A:Z"), INDEX(IMPORTRANGE("url", "SheetName!A:Z"),0,1)<>'')}
```

Do (in two separate sheets):

```
=IMPORTRANGE("url", "SheetName!A:Z")  // in Sheet1 of the new spreadsheet
=FILTER(Sheet1!A:Z, Sheet1!A:Z <> '')
```

In the Don't example, you are importing the range twice, which is very expensive. In the second example, you import the range once into a helper table, and then filter the result, thus importing only once.

Don't drag this formula down a column to calculate percent of time worked:

```
=A1/NETWORKDAYS(A1, EDATE(A1, 1)-1)
```

Do create a helper column in `B` with the result of `NETWORKDAY` and then in column `C` drag down

```
=A1/B1
```

#### Testing spreadsheet efficiency

In your spreadsheet, use `Ctrl+Shift+I` to open the developer tools. Under performance, record the spreadsheet loading (with data in it). Make a change to improve your formulas and reload the spreadsheet again. You can then see the performance improvement in ms. Note that this can be quite stochastic depending on your wifi at the exact moment, so run the test a few times for each configuration.

If still sluggish, consider deleting unused rows, columns in your spreadsheet, limit conditional formatting, or see other ideas [here](https://www.benlcollins.com/spreadsheets/slow-google-sheets/).

### Evolution of a formula

Like spreadsheets, formulas should be built iteratively. First, just make sure the formula does what you want it to do. Then start formalizing it such that it allows for user customization and is robust to changes in the spreadsheet.

[Example]

1. Write basic
2. Extract fixed values
3. Use named ranges
4. Substitute variables to allow extension (with caution)

## Basic Set Up

Although not necessary, it can be helpful to create a new folder on Google Drive to store all of your spreadsheets. 

We'll set up four spreadsheets, one for input values, two for data entry, and one for aggregation and data visualization.

Your folder should look like this:

[Insert drive image]

Now let's add some data. We'll support two different data entry formats to better illustrate the power of Google Sheets.

## Aggregating Data

You can support multiple 'rooms' as different sheets in the spreadsheet to make data entry easier for users. You must ensure that the rooms are identical (for the data you want to aggregate).

```
={Sheet1!A:Z; Sheet2!A2:Z>}
```

Note that the second and all subsequent ranges should exclude the headers. If you want to change the headers, you can use the first row to specify new header names and then in row 2 set the import to start at row 2 of Sheet1.

## Tidying data

Tidy data using the `UNPIVOT` custom function defined above and then filtering null values.

```
=UNPIVOT(Sheet3!A:E, 1, 1, "<parameter>", "<value>")  // in Sheet4!A1
=FILTER(Sheet4!A:E, Sheet4!A:A<>"")  // in Sheet 5!A1
```

Your data is now all tidied up and ready for export!

## Sharing data

The magic of Google Sheets is being able to treat multiple spreadsheets as a single system. [`IMPORTRANGE`](https://support.google.com/docs/answer/3093340) allows you to pull data together using the URL of each spreadsheet.

Once access is granted, any editor on the destination spreadsheet can use `IMPORTRANGE` to pull from any part of the source spreadsheet. The access remains in effect until the user who granted access is removed from the source.

If the data you are trying to import is too large, you may get an error.

### Managing connections

connectionLog

### Working in Shared Sheets

Filter Views

[Slicers](https://support.google.com/docs/answer/9245556?hl=en&ref_topic=9055396#zippy=%2Cfilters-filter-views-slicers)

