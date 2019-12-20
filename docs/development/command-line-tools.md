# Command Line Tools

Once you get comfortable with the command line, why not create your own utilities for quick tasks?

### The easy way

The easiest way to use the command line to run python programs is simply to write a script and run it from the CLI with python:

```
python <my_app.py>
```

If you don't want to provide the fully-qualified path to your script, open the command prompt from within the project's directory, or `cd` into it.

Nothing more to it than that.

### With arguments

If you need to pass arguments to your application (e.g., a file to save output), you'll need to be able to handle those arguments. The command line will pass anything you type after `python <my_app.py>` as strings to the program, but without a way of parsing and validating those strings, those arguments just get ignored.

There are a number of packages that you can explore for developing command line tools:

* `fire`
* `argpars`
* `click`

`fire` may be the easiest to use right out of the box, while `click` has more control for validation and providing help at the command line. Check out these tutorials on command line interface apps to get started:

* [How to write python command line interfaces like a pro](https://towardsdatascience.com/how-to-write-python-command-line-interfaces-like-a-pro-f782450caf0d)
* [Python command line tools](https://opensource.com/article/18/5/3-python-command-line-tools)



