# Getting Started

## Install python and basic packages

1. [Install miniconda](https://docs.conda.io/en/latest/miniconda.html)

2. Add to context menu (https://gist.github.com/jiewpeng/8ba446acf329b1801bf91db767d179ea)
   1. Run regedit.exe
   2. Navigate to `HKEY_CLASSES_ROOT > Directory > Background > shell`
   3. Add a key named `AnacondaPrompt` and set its value to "Anaconda Prompt Here" (or anything you'd like it to appear as in the right click context menu). Right click on the shell folder and select 'New Key'.
   4. Add a key under this key, called `command`, and set its value to `cmd.exe /K C:Users\Erik\miniconda3\Scripts\activate.bat` (may have to change the activate.bat file to where ever Anaconda is installed)

3. Add icon
   1. Create a new String Value in the `AnacondaPrompt` key (right click the key created in step 2.3) and set its value to the location of the `.ico` file you'd like to use, like `C:\Users\Erik\miniconda3\Menu\Iconleak-Atrous-Console.ico`

4. Install jupter lab
   1. Right click anywhere
   2. Open Anaconda Prompt
   3. Type `conda install -c conda-forge jupterlab`
   4. When prompted, type `y` to begin download

(You may need to upgrade some packages to get jupyter lab to run. Use `conda update <PACKAGE NAME>`, which will get you the latest version)

If you like, you can install a few additional often used packages into the base environment, but we recommend using environments for most projects, rather than installing everything into the base environment:

* `pandas`

### Why install packages into the base environment?

In general, we recommend creating [virtual environments](../development/virtual-environments.md) for most projects.

However, especially as you are getting started with coding, you'll want to be able to quickly explore different concepts. Having these commonly used packages installed in the base environment will allow you to explore without a lot of set up or tear down.

## Working in Jupyter Notebooks

Jupyter notebooks are a literate coding environment which allow you to mix code with your own text in Markdown. Outputs are printed directly to the screen. Notebooks are great for getting started, and may become an important part of your process even as you advance. I use notebooks for data exploration and when developing new processes. As I test different strategies and packages, I can capture both what works (which eventually finds its way into a `.py` file) and what *didn't* work. 

## Using Jupyter Notebooks with environments

As you are exploring a new project, you may want to use jupyter notebooks in a virtual environment. This will allow you to avoid polluting your base environment, or your project's main environment, with packages you won't end up using, and the jupyter notebook package itself. While you could install jupyter notebook directly in the environment (but make sure to complete step 6 below), you might not if you plan to deploy your app as you will end up installing jupyter notebook on the production server when you really don't need to. I like to set up a temporary environment for this exploration phase. Follow the process below in your command prompt.

1. `conda create -n <temp_env_name>`
2. `conda activate <temp_env_name>`
3. `conda install -c conda-forge <jupyterlab>`
4. (install any other packages needed)
5. `python -m ipykernel install --user`: this step installs the kernel into the environment so that your system can find the environment. See [here](https://jakevdp.github.io/blog/2017/12/05/installing-python-packages-from-jupyter/) for more on the relationship between a jupyter notebook kernel and the environment
6. `jupyter notebook` will launch the notebook in your browser (it will open automatically) 



