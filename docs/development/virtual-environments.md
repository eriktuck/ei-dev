# Virtual Environments

Virtual environments allow you to maintain a controlled development environment and are useful when deploying your application. They're actually quite easy to set up and use. See the [basic workflow](#virtual-environment-workflow) for a quick reference.

### Why use a virtual environment?

Imagine you have built an app using Python v2.7. You decide to upgrade your Anaconda distribution, which upgrades all of your packages and upgrades Python to v3.6. All of the sudden, all of your print statements return an error because they are missing parentheses! 

To avoid this, you can create virtual environments for each of your projects. A virtual environment simply saves all of the packages your using, as well as the python version (in some cases), in a unique folder rather than with the base Anaconda packages. This way, you can always be sure that the packages you're using aren't upgraded (or downgraded) in the future when you upgrade or install new packages to your base environment.

### The base environment

If you're not working in a virtual environment you have created, you're working in the base Anaconda environment (assuming you're working from an Anaconda distribution). You'll notice that every time you open an Anaconda Prompt window, it automatically activates the base environment with the command `conda activate base`.

When you `conda install` a package, it is stored in this base environment.

### Creating virtual environments

To create a virtual environment, simply use the command `conda create -n <name-of-environment> python`. If you want a specific python version, specify it as `python=3.6.3` for example. This environment is stored in Anaconda's environment directory (`C:\Users\User\Anaconda3\envs`), rather than the project's folder. Be sure to use the Anaconda Prompt window when creating virtual environments.

If you're not using Anaconda, you can use the `venv` or `virtualenv` packages (however they do not work with the Anaconda distribution.) `venv` is the preferred package and should work fine, unless you need to specify a python version, then use `virtualenv`.

### Working in virtual environments

To activate an environment, simply type `conda activate <name-of-environment>`. To deactivate, type `conda deactivate`. 

If you're using VS Code, you should tell it that you are working in a virtual environment so it can properly lint (highlight errors) based on the packages in your environment. Here's how:

1. Ctrl+Shift+P to open the command palette
2. Search `python: select interpreter`
3. Select the environment from the dropdown list

You will likely be prompted to install pylint into this environment, go ahead and do so using conda.

### Virtual environment workflow

Here's a quick workflow reference for setting up and working with virtual environments:

1. Open an Anaconda Prompt window
2. Create the environment: `conda create <name-of-environment>`
3. Activate the environment:  `conda activate <name-of-environment>`
4. Install necessary packages:  `conda install -c conda-forge <package-name>` (Check Anaconda's [package repo]( https://anaconda.org/anaconda/repo ) for the correct channel to install from)

5. Build your project
6. If you need to deactivate the environment, use `conda deactivate` or `conda activate base` to go back to the base environment (both have the same result).

For packages not on Anaconda's package repo, you may need to install pip before using a `pip install` command.

### Sharing virtual environments

Another great thing about virtual environments is that they are easy to share. And, if you're planning to deploy an app on a platform like Heroku, you must specify the environment (by specifying the necessary packages).

Virtual environments are shared with a `requirements.txt` or `environment.yml` file. Others can use this to recreate the environment on their own machines. To create this file, make sure the environment is activated and type `conda env export > environment.yml` (for Anaconda) or `pip freeze > requirements.txt` (if using pip). This file will be saved in the current working directory. It will include not just the top level libraries you are using (e.g., the libraries you import at the top of your script), but also the dependencies for the current version of those libraries.

### My 2-cents

I recommend creating a new environment for any long-lived project using `conda create -n <name-of-environment> python` and saving the name of the environment as the same as the root folder of the project. Install pip within it only if needed. The base conda environment (`conda activate base`) is useful for quick projects or ongoing-development type projects that you don't mind breaking from time to time, but plan to always keep up to date.

If you code a project without a virtual environment and don't come back to it for a year, there's a good chance you'll find the code doesn't work anymore, or worse yet behaves unexpectedly, and you'll need to go through and update it--which might be more trouble than it's worth. So use virtual environments to save you the hassle. At the very least, keep a `requirements.txt` or `environment.yml` file of the current state of the base environment for later reference.