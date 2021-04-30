# SQLAlchemy

SQLAlchemy is a python package that handles interfacing with databases. You can easily switch from SQLite as a test database to Postgres as a deployment database without changing any code. This tutorial will introduce you to [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/), an extension for [Flask](http://flask.pocoo.org/) which is configured for easier use with Flask apps (e.g., Dash apps). 

## Installation

```python
conda install -c conda-forge flask-sqlalchemy
```

Also install Flask if needed. If you're using Postgres (as we will in this tutorial), also install psycopg2:

```python
conda install -c conda-forge psycopg2
```

## Set Up

Circular imports can be a problem with `flask-sqlalchemy`, to avoid that we'll set up a package within our project to handle the namespaces. The project directory will look like this:

```
root/
| app/
 -- __init__.py
 -- models.py
 index.py
```

`index.py` will run the app. Within `app/`, `__init__.py` will set up the app, and `models.py` will contain the data definition language.

## Create an empty database

First we need to create the database. For this project, we'll use a Postgres database locally for testing and then again after we deploy to Heroku. See [this tutorial](postgres-tutorial) for the steps to create the Postgres database on your local machine. Follow steps 1 - 3, then return here.

## Configure the application

In your `__init__.py` file, copy/paste the following code:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# switch ENV variable on deployment.
ENV = 'dev'

if ENV == 'dev':
    app.debug = True
    app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:<password>@localhost/<database>'
else:
    app.debug = False
    app.config['SQLALCHEMY_DATBASE_URI'] = ''

# avoid warning in console
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
```

Update the `SQLALCHEMY_DATBASE_URI` string with your password and database name.

If instead you'd prefer to use a SQLite database, simply substitute `sqlite:///test.db` for the `SQLALCHEMY_DATABASE_URI` environment variable.

Note that the models package has not been configured yet, which we will do in the next section.

In your `index.py` file, paste the following code:

```python
from app import app

if __name__== '__main__':
    app.run()
```

You can run the app using `python index.py`. For now, there is nothing in the app to run.

## Define database structure

Database tables are created as python classes for easy access within the application. 

```python
from datetime import datetime  # used for timestamps
from app import db  # database object created in __init__.py

class Project(db.Model):
    __tablename__ = 'projects'
    id = db.Column(db.Integer, primary_key=True)
    project_name = db.Column(db.String(200), unique=True, nullable=False)
    
    def __repr__(self):
        return f'Project('{project_name}')

class HabitatOutcome(db.Model):
    __tablename__ = 'habitat_outcomes'
    id = db.Column(db.Integer, primary_key=True)
    date = db.Column(db.DateTime, default=datetime.utcnow)  # do not add parens to utcnow
    project_id = db.Column(db.Integer, db.ForeignKey('projects.id'), nullable=False)
    quantity = db.Column(db.Float, nullable=False)
    unit = db.Column(db.String(20), nullable=False)
    benefit_type = db.Column(db.String(100), nullable=False)
    
    def __repr__(self):
        return f'{quantity} {unit}s of {benefit_type}'
```

The class Project represents a table for projects with an auto-incrementing id as a primary key and project_name as the only attribute. Defining a string representation using the `__rep__` method is good practice to help with debugging. The class `HabitatOutcome` represents a quantity of benefit and is associated with projects through the `project_id` foreign key.

The table name in the database will be converted from the class `CamelCase` name to `camel_case` by default. Otherwise a table name can be specified with the`__tablename__` dunder method. I prefer to specify table names to have singular class names and plural table names. 

Note that both the unit field and the benefit_type field should have explicit options to enforce data integrity. Setting up tables to store options is discussed below, but for simplicity this tutorial will not set those up for now.

### Relationships

As we saw above, it is straightforward to specify relationships between entities with a foreign key. SQLAlchemy also allows us to store with the table a query which will access information about these relationships. For example we can extend the `Project` class to get habitat outcomes associated with a project from any instance of the class:

```python
class Project(db.Model):
    ...
    habitat_outcomes = db.relationship('HabitatOutcome', backref='project', lazy=True)
```

`habitat_outcomes` is not a column in the table `project` but rather a query that will return a list of all habitat_outcomes associated with a project. The query can be accessed as a property of each project with `project.habitat_outcomes`. The `backref` parameter will allow us to get the project associated with any habitat outcomes using `habitat_outcome.project'`. 

We can use the relationship in the string representation of the habitat outcome.

```python
class HabitatOutcome(db.Model):
    ...
	def __repr__(self):
    	return f'{quantity} {unit}s of {benefit_type} from {self.project.project_name}'
```

### Many-to-many relationships

If you want to use many-to-many relationships you will need to define a helper table that is used for the relationship. For this helper table it is strongly recommended to not use a model but an actual table. You will need to specify this table before the two tables it relates.

```python
projects_programs = db.Table(
    'projects_programs',
    db.Column('project_id', db.Integer, db.ForeignKey('projects.id', primary_key=True)),
    db.Column('program_id', db.Integer, db.ForeignKey('programs.id', primary_key=True))
)
```

Now we can include the programs associated with a project using a many-to-many relationship like this:

```python
class Project(db.Model):
    ...
    programs = db.relationship('Program', secondary=projects_programs, 
                               lazy = 'subquery',
                               backref=db.backref('projects', lazy=True))
```

Now, `project.programs` will list all of the programs associated with the project.

Of course, you need to add a table to store programs:

```python
class Program(db.Model):
    __tablename__ = 'programs'
    id = db.Column(db.Integer, primary_key=True)
    program_name = db.Column(db.String(200), nullable=False)
```

### Enumeration or option tables

When you want to restrict user inputs to a set of values, like data validation from a list in Excel, you should use tables to store those options. These should be specified as classes.

```python
class ConservationPlanningArea(db.Model):
    __tablename__ = 'conservation_planning_areas'
    id = db.Column(db.Integer, primary_key=True),
    cpa_name = db.Column(db.String(200). nullable=False)
```

When you specify the field in the parent table, the field will be an integer with a foreign key in the child table:

```python
class Project(db.Model):
    ...
    conservation_planning_area = db.Column(db.Integer, db.ForeignKey('conservation_planning_areas.id'), nullable=False)
```

## Create the database

Once the data structure is specified in `models.py`, creating the database is very easy. In the console, type:

```
python
from app import db  # app references app package, not the app
from app.models import *  # load all tables/classes into namespace
db.create_all()
```

Open pgAdmin and refresh the tables to see if the database was set up correctly. Note that re-running `db.create_all()` will only add new tables and will not override existing data, add new fields/relationships, or fail if tables already exist. Use `db.drop_all()` to delete all tables and start over.

## Adding data

Typically, you will add data programmatically through your application. If you have existing data to pre-populate your database, you may either batch upload `.csv` files (continue the [Postgres tutorial](postgres-tutorial)) or write a script to read and insert data. Once you've deployed your application, you'll want to insert data provided by users with functions in your code. 

Here we go over some of the basic commands for creating, reading, updating, and deleting data using SQLAlchemy. It will be up to you to adapt these concepts to your application. Here we'll insert and delete data to the local database instance. You may need to repeat these steps for your production database after setting it up. 

First, we'll need to add conservation planning areas to the database. This must be added first because we'll need to specify a conservation planning area when we add a project.

```python
feather_river = ConservationPlanningArea(cpa_name='Feather River')
db.session.add(feather_river)
db.session.commit()
```

Let's check that the area was added.

```python
ConservationPlanningArea.query.all()
>>>[<ConservationPlanningArea 1>]
```

We can access properties of the Project with the dot accessor.

```python
feather_river.id
>>>1
```

Now, let's add a project to the database. You must both add and commit the project to the database. 

```python
project_1 = Project(project_name='Credit Project A', conservation_planning_area=feather_river.id)
db.session.add(project_1)
db.session.commit()
```

If you add something incorrectly, rollback the session with `db.session.rollback()`.

We can also access records based on their ids:

```python
Project.get(1)
>>><Project 1>
```

Next, we'll add a few habitat outcomes.

```python
outcome_1 = HabitatOutcome(project_id=project_1.id, quantity=10, unit='acres', benefit_type='GGS')
outcome_2 = HabitatOutcome(project_id=project_1.id, quantity=200, unit='lineal feet', benefit_type='Shaded Riparian')
db.session.add(outcome_1)
db.session.add(outcome_2)
db.session.commit()
```

Now let's check the relationships we set up. First, we'll check that the habitat outcomes are indeed associated with the right project:

```python
project_1.habitat_outcomes
>>>
```

We can also access the project associated with each habitat outcome using the `backref` we set up:

```python
outcome_1.project
>>>
outcome_2.project
>>>
```



## Querying data

We've seen some basic queries above, including `.query.all()` and using dot accessors to access properties of each record. Here we'll look at some more advanced queries including filters, joins, aggregations, and pivots (cross-tab).



For advanced queries, you can submit any SQL statement with `db.session.execute("FROM projects SELECT *")`.  Try saving these queries in a `.sql` file, then passing the contents of that file to `execute()`. This is a good way to create views in the database, which you can then access with `db.session.execute("FROM view SELECT *")`.

Another tactic is to read a SQL statement into a `pandas` dataframe using `df = pd.read_sql()` and continue manipulation in pandas. I recommend using SQL to the extent possible, saving common data queries as views in the database, and reading those views into pandas. It is more performant, reduces duplication of code across applications serving the database, and a better starting place for your team. This is not supported out-of-the-box with SQLAlchemy, but see [here](https://stackoverflow.com/questions/9766940/how-to-create-an-sql-view-with-sqlalchemy) if you want more on how to create views and represent them in the ORM.

## Moving to Production

See the [Postgres tutorial](postgres-tutorial) for help setting up a database on Heroku, beginning with step 7. Once the database is set up and your code is deployed to Heroku, you can create the database tables with the command:

```python
heroku run python
from app import db
from app import models
db.create_all
exit()
```

 Connect to the database on pgAdmin and confirm the database was set up correctly.

To view data from the database on Heroku, you can follow this pattern:

```python
pg: psql --app <app_name>
select * from <table_name>
```



# Resources

Corey Shafer Flask app series

* [Package Structure](https://www.youtube.com/watch?v=44PvX0Yv368)



