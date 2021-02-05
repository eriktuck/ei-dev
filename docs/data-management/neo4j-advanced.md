# Neo4j Advanced

This topic contains advanced procedures for Neo4j graph databases.

## Neo4j admin

Neo4j-admin is the CLI for Neo4j. To access it in Neo4j Desktop, select 'Manage' from the three-circle menu in the top right of a database card. Then select the 'Open Terminal' button. You must prepend `bin\` to any calls to `neo4j-admin` for it to work. For example:

```
bin\neo4j-admin ...
```

## Dump and load database

If you need to make a copy of an existing graph database to share, you can use the `dump` and `load` commands with neo4j-admin.

First, open the project you want to share. Make sure the database is stopped. Click the three-circles button in the top right of the database card and select 'Manage'. Click 'Open Terminal'. In the Terminal, type:

```
bin\neo4j-admin dump --database=neo4j --to=<path\to\destination.dump>
```

Note that the default database for this project will be saved (I believe this is the database in the first position). If you use the name you gave a database in the --database flag, you'll get a database does not exist error. I have no idea why they set it up this way, but it is the way it is.

To load the database, first create an empty database in a new project. Then, following the process above, access the terminal and type:

```
bin\neo4j-admin load --from=<path\to\destination.dump> --database=neo4j --force
```

The `--force` flag will overwrite the existing database, which is why I recommend starting a new project so you don't overwrite something you want to keep.

See [here](https://neo4j.com/docs/operations-manual/current/tools/dump-load/) for more.

## Web hosting

Neo4j graph databases can be hosted in [Aura](https://neo4j.com/cloud/aura/) (Neo4j's own hosting service), on [AWS](https://aws.amazon.com/marketplace/pp/Neo4j-Neo4j-Graph-Database-Community-Edition/B071P26C9D) or deployed in [Docker](https://neo4j.com/developer/docker-run-neo4j/) (among other options). While Neo4j advertises pricing by the minute, in fact they bill by the month: you cannot stop an instance of an Aura graph database. One month of hosting will run about $64. Previously, GrapheneDB offered an option through Heroku for $9, however that is no longer supported.

### Create the database

Setting up a database on Aura is fairly easy. You'll need to set up an account and provide a credit card. Then, just hit 'Create Database'. Save the username and password into a `JSON` file in your `secrets/` directory. The URI is less sensitive, but might as well just keep it all together.

The code below will establish a connection using `neo4j`.

```python
from neo4j import GraphDatabase
import json 

with open('secrets/aura_creds.json') as f:
    creds = json.load(f)

URI = creds.get('URI')
USERNAME = creds.get('USERNAME')
PASSWORD = creds.get('PASSWORD')

graph = GraphDatabase.driver(URI, auth=(USERNAME, PASSWORD))
```

Note that it appears that `py2neo v2020.1.0` does not support the protocol used by Aura. If your app simply queries data from the database with cypher, `neo4j` will be sufficient. If you need more advanced functionality, and don't want to learn `neo4j` to that extent, check this [stack overflow topic](https://stackoverflow.com/questions/64880029/connect-to-neo4j-aura-cloud-database-with-py2neo) to see if anyone can help. Al

### Populate with existing database

To push an existing database to this cloud instance, follow the guidance under 'Import'. See the guidance [above](#neo4j-admin) for help using the terminal. Note that the database name is not the one you gave it, it is likely 'neo4j'.

## Unit Testing

As you populate your graph with data and your schema expands, you may want to set up unit tests to ensure that the results you're getting are accurate. You can set up a small graph with your initial schema but less data. Then write queries that you know the answers to. When you want to update the graph schema, first update the test graph schema and test that the right answers are returned. Write new tests to cover the extension of the graph schema you are planning.

## Importing Data

You'll often start a graph database by importing data from CSV files. You can also import JSON files from you API. CSV files can be loaded from a specific 'Import' file on your local directory, or they can loaded from a URL (Google Sheets, GitHub, Dropbox). 

To find the local folder, open the 'Manage' menu of your graph (three dots on the top right in the Desktop client, not the Browser). Click the split button 'Open Folder' and select 'Import'. A folder will open; drag and drop files into this folder. 

Alternatively, commit your project to GitHub and get the URL for the raw version of your data. Or save it in Google Sheets and get the public link. For step by step instructions, see [here](https://neo4j.com/developer/kb/import-csv-locations/).

Once you have your data available, use the `LOAD CSV` command to read the data into Neo4j from the local file and parse the data into the graph. Here's a simple example, but see [our tutorial] or the [documentation](https://neo4j.com/developer/guide-import-csv/) for more.

Here's the first two rows of the CSV (avoid spaces in header names):

| Name | Role             | Rate |
| ---- | ---------------- | ---- |
| Jim  | Senior Associate | $150 |
| Jan  | Associate        | $100 |

Here's how to load entities into a graph database. Employees and Roles are represented as nodes. The role for each employee will be expressed as a relationship after nodes are initially loaded. You'll copy this Cypher code into the terminal of the Neo4j Browser application:

```cypher
LOAD CSV WITH HEADERS FROM 'file:///data.csv' AS row
MERGE (person:Person {name: row.Name})
MERGE (role:Role {role:row.Role, rate:row.Rate})
```

In this statement, the CSV is first made available to the application. Each row will be iterated through, accessed with the `row` variable. Using the `MERGE` command, we assign the contents of the 'Name' column for that row (`row.Name`) to a node labelled 'Person' (`person:Person`), specifically to the property 'name' (`{name: row.Name}`). The `MERGE` command ensures no duplicates are created, `CREATE` could also be used but might result in duplication.

Next, assign roles to employees through relationships:

```cypher
LOAD CSV WITH HEADERS FROM 'file:///data.csv' AS row
MATCH (person:Person {name: row.Name})
MATCH (role:Role {role: row.Role})
MERGE (person)-[:HAS_ROLE]->(role)
```

Again, the CSV is made available to the application. We'll need it to see which person has which role. The two `MATCH` statements identify nodes within our graph that have the same name and role as the first row in the CSV. With the correct Person and Role nodes identified, a new relationship `HAS_ROLE` is created with the `MERGE` command, again to avoid duplication.

Continue in this way until all entities are imported as nodes and all relationships are defined between nodes.

### Working with files

The [Neo4j tutorial](neo4j-tutorial.md) provides in depth guidance for translating a relational database to a graph database using csvs. Below we provide some basic guidance on working with files, especially csvs.

Neo4j does not allow you to access files on your local system. Instead, you must upload files into the project's file directory before they can be available to Neo4j. Add a file using the 'Add File' card in the Desktop client. Of course, changes made to the file in your local directory will not be reflected in this copy, you must upload any changes directly.

To use the file in Neo4j, click on the three-circle icon in the top right of the uploaded file card and select 'copy file URL'.

Use the copied file url like this:

```cypher
LOAD CSV WITH HEADERS FROM "<paste url here>" as row 
...
```

For example, you can load entities from two columns 'Project_Number' and 'Region' with:

```cypher
LOAD CSV WITH HEADERS FROM "http://localhost:11001/project-5810fc37-0742-4c0b-b0d7-238646cc50ea/reports.csv" as row
MERGE(p:Project {name: row.Project_Number})
MERGE(r:Region {name: row.Region})
RETURN p, r
```

To create relationships between these nodes (where any nodes in the same row are related):

```cypher
LOAD CSV WITH HEADERS FROM "http://localhost:11001/project-5810fc37-0742-4c0b-b0d7-238646cc50ea/reports.csv" as row
MATCH(p:Project {name: row.Project_Number})
MATCH(r:Region {name: row.Region})
MERGE(p)-[:LOCATED_IN]->(r)
```

