# Py2neo

Py2neo is a python package for working with graph databases on Neo4j. It's not officially supported by Neo4j (as the package `neo4j` is), but it offers a worthwhile suite of tools for working with graph databases in a more pythonic way. 

Before you get too invested, a word of warning. The package is not as well used as something like `pandas`, and it has very little in the way of usage documentation--just a straightforward documentation of its API. You will struggle to find current documentation  for problems your facing (besides this wonderful guide, of course). When you do find a solution, it will almost certainly be out of date and no longer work. There's almost nothing on YouTube posted in the last 5 years. You will probably end up digging through its source code at some point trying to figure something out. If you're still here, read on!

!!! Tip
    Always start at the home page of the py2neo handbook ([py2neo.org](https://py2neo.org/)) when looking for documentation. If you search google for the specific function or issue you are having, the documentation will be out of date.

## The missing quick start guide

To get up-to-speed, we'll work through a quick canonical example.

### 0. Installation

To install the latest release of py2neo, simply use:

```
pip install --upgrade py2neo
```

### 1. Create a graph database

First, you'll need to create an empty graph database using Neo4j Desktop. Check out the [Neo4j tutorial](../data-management/neo4j-tutorial.md) for the first steps. Start the database. The database should be exposed on port 7687. If not, you will have to update the URI in the next step. Make sure you remember the password.

### 2. Connect with py2neo

Once the graph database has been started, it will be available to connect.

```python
from py2neo import Graph

URI = 'bolt://localhost:7687'
USERNAME = 'neo4j'
PASSWORD = '<password>'

graph = Graph(URI, auth=(USERNAME, PASSWORD))
```

The first argument to graph is the URI string for the graph. The `auth` parameter is a tuple of the username (generally 'neo4j') and your password.

### 3. Create nodes and relationships

It is quite easy to create nodes and relationships. 

```python
from py2neo import Node, Relationship

a = Node("Person", name="Alice", age=33)
b = Node("Person", name="Bob", age=44)
KNOWS = Relationship.type("KNOWS")
graph.merge(KNOWS(a, b), "Person", "name")
```

The nodes assigned to `a` and `b` and the relationship `KNOWS` are not actually created until the `merge` command is called.

Let's look at an alternative pattern that uses a transaction to create the nodes and relationships. This is going to be safer in most cases, as a transaction will be rolled back if any part fails.

```python
from py2neo import Node, Relationship

tx = graph.begin()

a = Node("Person", name="Alice")
tx.create(a)

b = Node("Person", name="Bob")
ab = Relationship(a, "KNOWS", b)
tx.create(ab)

tx.commit()
```

Note some commands are transactions, and you shouldn't use both at the same time (e.g., update properties of node with graph.push()). If you have up to 20,000 operations to run you can batch those with one transaction. This is both safe and efficient. Over 20,000 you may run into a memory issue and you'll need to periodically commit as you are running (committing each of significantly more than 20,000 transaction might take a long time).

See more on [adding nodes](#add-nodes).

### 4. Query graph

You can run any cypher query with the `graph.run()` command:

```python
results = graph.run("MATCH(p:Person {name='Alice'})-[k:KNOWS]-(p2) RETURN p2.name")
```



## Usage

### Add nodes

A node has:

* a label (e.g., 'Person'), 
* a suite of properties (e.g., name='Alice'), 
* a primary key (the property with which to identify the node, e.g., 'name'), and 
* a primary label (e.g., 'Person')

```python
tx = graph.begin()
new_node = Node('<LABEL>', <id_property>=<>,
                primary_key='<id_property>',
                primary_label='<LABEL>')
tx.merge(new_node, '<LABEL', '<id_property>')
tx.commit()
```

You can also use a dictionary to add properties. Any kwargs are read as properties, any args are read as labels. This can be helpful when reading from a pandas dataframe (see Import from DataFrame rows below).

```python
property_dict = {
    '<property1>': '<prop_val1>',
    '<property2>': '<prop_val2>'
}

new_node = Node('<LABEL>', <id_property>=<>,
                primary_key='<id_property>',
                primary_label='<LABEL>',
               **property_dict)
```

### Add relationships

If 

### Match Nodes

To match nodes, first set up a matcher object:

```python
matcher = NodeMatcher(graph)
```



### Update properties (one property)

This runs as a transaction, so don't wrap in transaction. Note that updates to properties occur only locally until pushed using `graph.push`.

```python
node = matcher.match('<LABEL>', <id_property>=<>).first()
if node:
    node[<property>] = <>
    graph.push(node)
```

### Update properties (multiple properties)

```python
node = matcher.match('<LABEL>', <id_property>=<>).first()
if node:
    node.update(**properties)
    graph.push(node)
```

### Delete nodes from list

```python
tx = graph.begin()
for node in bad_node_list:
    node_matches = matcher.match('<LABEL>', <id_property>=node)
    for node in node_matches:
        graph.delete(node)
tx.commit()
```

### Query Graph

[Cypher Query Cheatsheet](https://gist.github.com/DaniSancas/1d5265fc159a95ff457b940fc5046887)

To get data from the graph

```python
results = graph.run("<CYPHER QUERY>")  # returns cursor to stream results
for result in results:
    # do something
```

Instead of streaming results, data can be read to a list of dictionaries 

```python
results = graph.run(
    f"""
    MATCH(n:Node)-[]-()
    WHERE n.name = "{<name>}"
    RETURN n.name, n.prop_1, n.prop_2
    """
).data()
# returns:
[
    {'n.name': '', 'n.prop_1': '', 'n.prop_2': ''},
    {'n.name': '', 'n.prop_1': '', 'n.prop_2': ''},
    ...
]
```

If your graph has spaces in the properties, use indexing:

```python
results = graph.run(
    f"""
    MATCH(n:Node)-[]-()
    WHERE n.name = "{<name>}"
    RETURN n['my name'], n['my prop_1'], n['my prop_2']
    """
).data()
```

If labels have spaces, use backticks 

```python
results = graph.run(
    f"""
    MATCH(n:`My Node`)-[]-()
    WHERE n.name = "{<name>}"
    RETURN n['my name'], n['my prop_1'], n['my prop_2']
    """
).data()
```

If you need relationships from one central node to multiple other nodes, use OPTIONAL MATCH:

```python
results = graph.run(
	f"""
	MATCH(n:Node)-[]-(o:other_node)
	WHERE n.name="{<name>}"
	OPTIONAL MATCH (o)-[]-(p)
	WHERE p:<Label1> OR p:<Label2>
	RETURN n.name, o.name, labels(p), p.name
	"""
).data()
```

This query gives you nodes with labels Label1 or Label2 related to node with label node that is connected through other_node. Note that in the above example the identifying property for all additional nodes must be the same, namely `name`.



### Working with pandas

Py2neo integrates well with pandas.

#### Export to DataFrame

Note that nodes may not conform well to pandas expectations, and unexpected errors can occur.

```python
df = graph.run("MATCH (a:Person) RETURN a.name, a.born").to_data_frame()
```

#### Import nodes from DataFrame columns

If lists of nodes of the same type are stored in DataFrame columns, you can create a unique list from each column and create a node using that list with the label as the column header. Labels may contain spaces.

```python
tx = graph.begin()
for column in df.columns:
    node_set = df[column].unique().astype(str)  # some data types not supported as labels
    for node in node_set:
        new_node = Node(column, <id_property>=node,
                       primary_key='<id_property>',
                       primary_label=column)
        tx.merge(new_node, column, '<id_property>')
tx.commit()
```

https://stackoverflow.com/questions/45738180/neo4j-create-nodes-and-relationships-from-pandas-dataframe-with-py2neo

#### Import nodes from DataFrame rows

Alternatively, if the nodes correspond to rows and columns are properties (you should find this with tidy datasets), read the df with the primary label as the index column, convert to a dictionary, and iterate through the dictionary items to add nodes. Note that the index column must be unique.

```python
df = pd.read_csv('path/to/data', index_col='<id_property>')
node_dict = df.to_dict('index')

tx = graph.begin()
for node, properties in node_dict.items():
    node = Node('<LABEL>', <id_property>=node,
                primary_key='<id_property>',
                primary_label='<LABEL>',
                **properties)
     tx.merge(node, '<LABEL>', '<id_property>')
tx.commit()
# needs to be tested
```

Using `**properties` passes the dictionary of properties, where each column is a key and the data in the cell is a value, to the Node object.

#### Add relationships from DataFrame columns

Where nodes are stored in columns, and nodes have already been imported, you can use either `df.iterrows()` or convert the DataFrame to a dictionary to relate all nodes in a single row. 

```python
from py2neo import NodeMatcher
matcher = NodeMatcher(graph)

df = pd.read_csv('path/to/data', index_col='<id_property>')
entity_dict = df.to_dict()

tx = graph.begin()
    for node_label, node_dict in entity_dict.items():
        for project_id in entity_dict[node_label]:
            project_node = matcher.match('Project', 
                                         project_number=project_id).first()
            entity_node = matcher.match(
                node_label, name=node_dict.get(project_id)).first()
            if project_node and entity_node:
                relationship = Relationship(project_node, "IN", entity_node)
                tx.create(relationship)
        
    tx.commit()
```

How this works:

The `entity_dict` looks like:

```python
{
    'Country': {'AID-512-A-00-08-00005': 'Brazil', 
             	'AID-512-A-00-08-00015': 'Brazil', 
             	'AID-512-A-10-00004': 'Brazil', 
             	'AID-512-A-11-00004': 'Brazil', 
             	'AID-512-A-16-00001': 'Brazil'}, 
 	'Income Group': {'AID-512-A-00-08-00005': 'Upper Middle Income Country', 
                  	'AID-512-A-00-08-00015': 'Upper Middle Income Country', 
                  	'AID-512-A-10-00004': 'Upper Middle Income Country', 
                  	'AID-512-A-11-00004': 'Upper Middle Income Country', 
                  	'AID-512-A-16-00001': 'Upper Middle Income Country'}
}
```

Each column has the relationship required between the index (in this case a project number) and the node of the type contained in that column. You can create all of the relationships required and then repeat the process by specifying a new index column, if needed.

For each column (node_label), we use the dictionary associated to match each project id and each node. If a match is found for both, we create a relationship. Don't forget to commit the transaction.

You can use another dictionary to specify the label for the relationship if you want to have different relationship labels for different columns. Simply lookup the relationship name in place of "IN".

#### Using objects

```python
from py2neo.ogm import GraphObject, Property
class Person(GraphObject):
	name = Property()
	born = Property()
	
[(a.name, a.born) for a in Person.match(graph).limit(3)]

# returns
[('Laurence Fishburne', 1961),
 ('Hugo Weaving', 1960),
 ('Lilly Wachowski', 1967)]
```

#### 