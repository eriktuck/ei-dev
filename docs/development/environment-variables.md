# Environment Variables

Environment variables allow you to use sensitive or secret information (credentials, passwords, tokens, etc.) in a script without hard-coding those values into your script. Typically, you won't want to store something like a password as a string directly in your code. If you commit that code to GitHub it becomes a permanent part of your code's history and thus accessible to anyone who accesses your project on GitHub. There are web crawlers that sift through GitHub repositories looking for information like this, so be careful.

### Setting Environment Variables

Environment variables are set differently on different operating systems. On Windows, just type 'environment variables' into the start menu and open the System Properties dialog box ('Advanced' tab). Click on 'Environment Variables' to edit. Adding as a user variable, rather than a system variable, is generally sufficient. Conventionally, both the name and the value are uppercase.

![set-environment](assets/set-environ-var.gif)



#### Saving Environment Variables in a conda environment

You can set up [environment variables](environment-variables.md) within a conda environment. See [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#setting-environment-variables). 

### Using Environment Variables

Python reads the environment variables when `os` is imported. Environment variables are passed as a dictionary. Since this is simply a dictionary, you can use `os.environ.get('<KEY>')` to access the value with the name of the variable (where `'<KEY>'` is the name you specified for the environment variable). For example:

```python
import os

my_password = os.environ.get('PASS_KEY')
```

#### Using environment variables in Heroku

In Heroku, environment variables are set under 'Settings' by clicking 'Reveal Config Vars'. These are accessed in exactly the same way as above.

### A caveat

Environment variables are commonly used, but [not always the best option](https://blog.dnsimple.com/2018/05/using-environment-as-configuration/ ) for storing secrets. They can be subject to mangling by the operating system, and some access tokens don't conform to their limitations (e.g., my google access token had a '=' character, which is not allowed). Especially if your secrets are stored as a JSON blob, you might consider saving them in a `JSON` file and reading them directly from the file. 

### For the A/V Crowd

This video captures the essentials of Environment Variables in 5 minutes. Worth a watch for anything I missed.

<iframe width="560" height="315" src="https://www.youtube.com/embed/IolxqkL7cD8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

