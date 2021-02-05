# Neo4j Overview

This section is a high-level summary of content in the Neo4j [Developer Guides](https://neo4j.com/developer/get-started/); links are provided to relevant guides in each section. Also see these [resources](https://neo4j.com/developer/getting-started-resources/) (the [O'Reilly Graph Databases book](https://neo4j.com/graph-databases-book/) (free) is a must-read).

## Installation & Set Up

Download the latest [Neo4j Desktop](https://neo4j.com/download/) release for your operating system. Install as normal. Make sure you meet the minimum system requirements (esp. check that PowerShell is v5 or above). The following sections explain some of the key features of Neo4j and link to set up instructions.

## Solutions

#### Neo4j Desktop

Open the Neo4j Desktop client on your computer. Follow these [instructions](https://neo4j.com/developer/neo4j-desktop/) to provide your activation key and orient yourself to the client. One important note: you'll want to open the '[Manage](https://neo4j.com/developer/neo4j-desktop/#desktop-manage-db)'  menu on the database to locate the import folder (Neo4j can only read files within this folder, it can't access files in your project's directory) and install plugins.

#### Neo4j Browser 

You'll interact with graph databases in the Browser tool. 'Start' a graph database (or 'Add New Database') and, once it's ready, select 'Open'. This will open the Brower tool. Read through these [instructions](https://neo4j.com/developer/neo4j-browser/) for an orientation.

#### Neo4j Bloom

If you are going to be sharing your graph with a non-technical audience, [Bloom](https://neo4j.com/developer/neo4j-bloom/) is a great tool for making your graph available and defining helpful searches. Your audience will need to download Neo4j Desktop and have access to your graph, and have some training on the interface. However, once set up it's a great way to explore the graph manually or execute pre-defined searches.

#### APOC

APOC stands for Awesome Procedures on Cypher. (Cypher is the SQL-like language used by Neo4j). You'll need to install APOC and provision it for your database (see [instructions](https://neo4j.com/developer/neo4j-apoc/)). APOC contains many of the Natural Language Processing (NLP) algorithms you'll want (see [more](https://neo4j.com/developer/nlp/)).

#### Graph Algorithms

The Graph Algorithms library provides access to many graph algorithms for exploring and quantifying the graph. Check out the free book [O'Reilly Graph Algorithms](https://neo4j.com/graph-algorithms-book/). This library is downloaded and provisioned similar to APOC ([instructions](https://neo4j.com/developer/graph-algorithms/)).

#### Graph Data Science Playground

The Graph Data Science Playground provides a streamlined experience for executing graph data science algorithms against your graph. First install it by selecting the split button 'Open' and selecting 'Graph Apps Gallery'. Find the card for Graph Data Science Playground and click install (or paste the URL in the install bar as described [here](https://medium.com/neo4j/introducing-the-graph-app-gallery-81aa3e63567b)). The Playground will now be available under the 'Open' split button.

#### Aura

Neo4j provides a graph hosting service 'Aura'. Fees are advertised by the second, but in actuality graph instances cannot be stopped, so the cheapest option comes to $64.80 per month. It's fairly easy to set up and [upload an existing graph database](#making-a-copy). Instances can only be connected to with neo4j (not py2neo), so keep that in mind before selecting this option for hosting. Graphs can also be hosted on [AWS](https://aws.amazon.com/marketplace/pp/Neo4j-Neo4j-Graph-Database-Community-Edition/B071P26C9D) or deployed in [Docker](https://neo4j.com/developer/docker-run-neo4j/).

## Cypher

Cypher is the SQL-like language that Neo4j uses to add data and query graph databases. It's helpful but not necessary to have experience with SQL before learning Cypher. The free book [O'Reilly Graph Databases](https://neo4j.com/graph-databases-book/) is the best way to start learning Cypher, then check out the tutorials [here](https://neo4j.com/developer/cypher-query-language/). 

Cypher is more like a drawing using ASCII characters than a language per se. Entities are represented in parentheses `(jim:Person)`, properties are listed in braces `(jim:Person {name:'Jim'})`, and relationships are represented by square brackets between an arrow (`-[:OWES]->`). Relationships can also have properties `-[:CONTAINS {amount:50}]->`. 

So, the following statement would state that Jim owes Jan $50:

`(jim:Person {name:'Jim'}) -[:OWES {amount: 50}]-> (jan:Person {name: 'Jan'})`

*Note that the `jim` and `jan` before the `:Person` are optional in this statement, but would be required if returning one or both of the entities in a query.*

Use `CREATE` or `MERGE`to create that relationship in the graph  (best practice is to use `MERGE` to avoid duplication). Use `MATCH` to find that relationship in the graph. Cypher also has commands `WHILE`, `WITH`, `SET`, `UNION`, `FOREACH`, `CREATE UNIQUE`.

Use `RETURN` to get the results of a query.

## Data Modelling

The great thing about graph databases is that the schema is almost identical to the way we originally whiteboard out the information design. Unlike relational databases, you don't need to go through the normalization process and insert additional complexity, as when creating many-to-many lookup tables. 

![img](https://dist.neo4j.com/wp-content/uploads/matrix_whiteboard_model2.png)



First, describe in words the system you want to model. The 'nouns' in your system (Jim, Denver, desk) will be represented as *nodes*. Nodes are *labelled* as the type of entity they represent (people, places, things). Think about how your entities are connected (or identify verbs in your system); these are *relationships*. People 'know' other people. People 'work on' projects. Projects 'belong to' business units.

Assign *properties* to each node or relationship to capture attributes. Attributes often answer questions you have of each entity or relationship ('how old is Jim?', 'when did Jim move to Denver'?; add an attribute 'age' to the 'Person' node and an attribute 'move date' to the 'LIVES_IN' relationship).

A few additional considerations (see the Data Modeling chapter of Graph Databases):

* Use relationship attributes to express strength, weight, or quality of a relationship, plus any necessary metadata like timestamps, version numbers, etc.
* Use multiple relationships between nodes to express fine-grained and generic relationships. For example, one user may have a different billing and delivery address. Create two relationships to each, one generic `ADDRESS` and one more specific (`BILLING_ADDRESS`) or (`DELIVERY_ADDRESS`). You can then search for either address or both very efficiently. This is better than using relationship attributes for the same purpose, because it is faster to search.
* Use reciprocal relationships for bi-directional relationships. In other words, create  a relationship going from node `a` to node `b`, and the same relationship from `b` to `a`. When searching, it's best to omit the arrow in the relationship if the direction is not known or is not important; however, relationships cannot be defined without a direction.
* Add relationships when use cases dictate. For example, while you could find everyone that worked together in a company by traversing from people through projects to other people, you can also make this relationship explicit by including a `-[:WORKS_WITH]->` relationship. This will make queries faster and easier to write.
* Use nodes to model facts. For example, a person may work in one role for some time, and then be promoted to another role. Use a `Job` node to model the time that each person spends in each role. A job will have a start and end date as attributes, and be related to a role. (If job history is not important, you can simply delete the current relationship to a role and update with a new one.)

Once you have a general idea of the entities and relationships, think through the use cases for the graph. Write user stories if helpful (*As User X, I want Y so I can Z*).  Make sure that your schema will allow you to answer the questions that you'll need to answer to satisfy your use cases.

You don't need to have a fully built out schema to begin, in fact one of the benefits of graph databases is that the schema can grow (i.e., you can add entities and relationships) over time. However, you will want to be thoughtful about your design so that it is efficient and captures the relationships you need to capture.

For more detail, see Neo4j's [modeling designs guide](https://neo4j.com/developer/modeling-designs/).

### Naming Guide

* Node labels are `CapWords`
* Relationships are `ALL_CAPS`
* Properties, variables, parameters, aliases and functions are `camelCase`

## Visualizing & Sharing 

Graphs are so intuitive that it's tempting to share the graph itself with users so they can explore it. Sometimes you just need to make the graph available without exposing the graph itself. Neo4j offers a few options for sharing the power of graphs.

### Schema

To get the schema, use the command

```cypher
call db.schema.visualization()
```

You can export this as an image (see next header).

### Image

You can export a PNG or SVG of the graph from the Browser client by querying the entire graph (or a subset) and using the drop-down menu above the result. To get the entire graph, use the cypher command:

```cypher
MATCH (n)-[r]-() RETURN n, r
```

### Bloom

[Bloom](https://neo4j.com/developer/neo4j-bloom/) is installed alongside the Neo4j Desktop client and provides great functionality for exploring the graph in visual format. You can also define searches for users to help them get the information they need. Once you have a graph populated, use the split button 'Open' to open with Bloom instead of the Browser.

### GraphGists

[GraphGists](https://portal.graphgist.org/about) are similar to Jupyter Notebooks, but for graphs. They load the graph into the browser itself, so no back-end is required. However, they can only support small graphs (150 nodes and edges). Use these if you want to provide somewhat interactive graphs in a report or presentation.

### Kumu.io

[Kumu.io](https://kumu.io/) provides a platform for sharing graphs and system maps with some interactivity. You can display node information in a sidebar and 'focus' on specific nodes and relationships by clicking. It's a great way to explore systems maps, situation models, and other complex maps. Check out this [system analysis from The Nature Conservancy](https://kumu.io/tnc/tnc-global-situation-analysis-v2) for an example.

### Heroku and GrapheneDB

!!! Warning
    GrapheneDB as a low-cost solution has been deprecated.

You can deploy your graph database powered application to the web with Heroku and the [GrapheneDB](https://devcenter.heroku.com/articles/graphenedb) plugin. To get more than 1,000 nodes and 10,000 relationships, you'll need to pay the monthly fee for GrapheneDB ($9/month and up). To get some experience with deploying on Heroku, check out the [Movies Database tutorial](https://neo4j.com/developer/example-project/) (DO NOT USE THE `py2neo` repo they have listed, a [new repo](https://github.com/neo4j-examples/movies-python-py2neo) was created for the more recent version of py2neo--I have also found issues with that, contact me for a working copy).