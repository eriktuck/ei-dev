# Virtual Environments

Virtual environments allow you to maintain a controlled development environment and are useful when deploying your application. They're actually quite easy to set up and use. See the [basic workflow](#virtual-environment-workflow) for a quick reference.

### Why use a virtual environment?

Imagine you have built an app using Python 2.7. You decide to upgrade your Anaconda distribution, which upgrades all of your packages and upgrades Python to 3.6. All of the sudden, all of your print statements return an error because they are missing parentheses! 

To avoid this, you can create virtual environments for each of your projects. A virtual environment simply saves all of the packages you're using, as well as the Python version (if specified), in a unique folder rather than with the base Anaconda packages. This way, you can always be sure that the packages you're using aren't upgraded (or downgraded) in the future when you upgrade or install new packages to your base environment.

### The base environment

If you're not working in a virtual environment you have created, you're working in the base conda environment (assuming you're working from an Anaconda distribution). You'll notice that every time you open an Anaconda Prompt window, it automatically activates the base environment with the command `conda activate base`.

When you `conda install` a package from the base environment, it is stored in this base environment.

### Creating virtual environments

To create a virtual environment, simply use the command `conda create -n <env_name>`. If you want a specific python version, specify it (for example: `conda create -n <env_name> python=3.8`). This environment is stored in Anaconda's environment directory (`C:\Users\<User>\Anaconda3\envs`), rather than the project's folder. Be sure to use the Anaconda Prompt window when creating virtual environments.

If you're not using Anaconda, you can use the `venv` or `virtualenv` packages (however they do not work with the Anaconda distribution.) `venv` is the preferred package and should work fine, unless you need to specify a python version, then use `virtualenv`. For more info on those packages, see their documentation.

### Working in virtual environments

To activate an environment, simply type `conda activate <env_name>`. To deactivate, type `conda deactivate`. 

If you're using VS Code, you should tell it that you are working in a virtual environment so it can properly lint (highlight errors) based on the packages in your environment. Here's how:

1. Ctrl+Shift+P to open the command palette
2. Search `python: select interpreter`
3. Select the environment from the dropdown list

You will likely be prompted to install pylint into this environment, go ahead and do so using conda.

### Virtual environment workflow

Here's a quick workflow reference for setting up and working with virtual environments:

1. Open an Anaconda Prompt window
2. Create the environment: `conda create -n <name-of-environment> [python=3.8]` (note the square brackets denote an optional parameter, if you want to include a python package, )
3. Activate the environment:  `conda activate <name-of-environment>`
4. Install necessary packages:  `conda install -c conda-forge <package-name>` (Check Anaconda's [package repo]( https://anaconda.org/anaconda/repo ) for the correct channel to install from)
5. Build your project
6. Export the environment to an `environment.yml` file: `conda env export > environment.yml`
7. If you need to deactivate the environment, use `conda deactivate` or `conda activate base` to go back to the base environment (both have the same result).

For packages not on Anaconda's package repo, you may need to install pip before using a `pip install` command. Once you start using `pip`, don't go back to using `conda` to install packages. Do not use pip in the base environment. You'll note that the `environment.yml` file will specify which packages must be installed using `pip`.

### Sharing virtual environments

Another great thing about virtual environments is that they are easy to share. And, if you're planning to deploy an app on a platform like Heroku, you must specify the environment (by specifying the necessary packages in a `requirements.txt` file).

#### Exporting a virtual environment

Virtual environments are shared with a `requirements.txt` or `environment.yml` file. Others can use this to recreate the environment on their own machines. To create this file, make sure the environment is activated and type `conda env export > environment.yml` or `pip freeze > requirements.txt`. This file will be saved in the current working directory. 

Generally, you should save your environment in an `environment.yml` file since we'll be using conda environments most of the time. Heroku, however, requires a `requirements.txt` file. It's ok to have both in a project.

If you know you will be working on the same operating system (same machine or another machine), you can create an exact replica of the environment using `conda list --explicit > environment.yml`. This will ensure that all dependencies are of the same version. This likely won't work if you are recreating the environment on a different OS version (e.g., Windows 10 vs. 8.1).

If instead you may be working on completely different operating systems (e.g., Windows vs. OSx), you can be extra cautious and use `conda env export --from-history > environment.yml`. This will only include packages specifically installed into the environment, and allow conda to resolve any dependencies.

#### Duplicate virtual environment from file

To duplicate a virtual environment from the `environment.yml` file, use the command `conda env create -f environment.yml`. This will create the environment with the same name as the original environment. 

To duplicate a virtual environment from the `requirements.txt` file, first create an empty conda environment `conda create --name <env name>` , install pip `conda install pip` and then install the packages from the file `pip install -r requirements.txt`.

#### Cloning an environment

To clone an environment, simply `conda create --name <new_env_name> --clone <old_env_name>`.

#### Update a virtual environment

You can add packages to the environment simply by using `conda install <package>` or `pip install <package>` as you're building the project. Make sure to export the environment after installing new packages so that it's always up-to-date!

If changes were made to the environment by a collaborator or on a cloned project, you can install the new packages from the updated `environemnt.yml` file with the command `conda env update --prefix ./env --file environment.yml --prune`. The `--prune` option causes conda to remove old dependencies.

### Common issues

If you are trying to recreate an environment created on another OS platform (e.g., Windows vs. OSx, Windows 10 vs. 8.1) from an `environment.yml` file, the specific dependencies may not be available on your platform. In this case, go back to the original project and use `conda env export --from-history > environment.yml`. This will create a file using only the packages specifically imported (excluding any dependencies) and conda will resolve those dependencies on its own. Note that the two environments might not be identical, but it's better than not having the environment at all!

For a quick fix, try removing the version number from the packages in the `environment.yml` or `requirements.txt` file, and/or deleting packages you know to be dependencies (i.e., `numpy` is a dependency of `pandas`, only list `pandas` in the file). 

### When to create an environment

I recommend creating a new environment for any long-lived project using `conda create -n <env_name>`, saving the name of the environment as the same as the root folder of the project. Install as many packages as possible with conda, then install pip and use pip as the package manager (see [here](https://www.anaconda.com/blog/using-pip-in-a-conda-environment) for why). 

The base conda environment (`conda activate base`) is useful for quick projects or ongoing-development type projects that you don't mind breaking from time to time.

Always keep the `environments.yml` file up-to-date! Anytime you install a package, export the environment.

If you code a project without a virtual environment and don't come back to it for a year, there's a good chance you'll find the code doesn't work anymore, or worse yet behaves unexpectedly, and you'll need to go through and update it--which might be more trouble than it's worth. So use virtual environments to save you the hassle and keep the `requirements.txt` or `environment.yml` file up to date in the root folder of the project.

### More commands

To see your environments: `conda env list`.

To see packages in an environment: `conda list -n <env_name>` or simply `conda list` if the environment is active.

To remove an environment: `conda remove -n <env_name> --all`.

To update conda: `conda update conda`.

To update a package: `conda update <package>`.

To change python version: `conda install python=2.7` (make sure the environment is activated or this will downgrade/upgrade python system-wide!)

You can set up [environment variables](environment-variables.md) within an environment (although this is not my preferred way of handling [secrets](secrets.md)). See [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#setting-environment-variables).

### Resources

[CONDA CHEATSHEET](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)