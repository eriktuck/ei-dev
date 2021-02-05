# Getting Started

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