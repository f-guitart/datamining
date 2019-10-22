# Data Processing Envirments with Python

## What you will learn in this Laboratory
* What are the main basic tools to process data in a computer
* What are the most used environments within the python data community
* What are the differences between the proposed environments
* When to use each of the proposed environments
* Configure and run different enviroments
  
## Outline
1. What is a Data Processing Environment
2. Overview of the different Python environment configurations
   1. Python Virtual environments
   2. Pipenv
   3. Conda
   4. Docker
3. Configuring a Pipenv environment
4. Configuring a Conda environment
5. Configuring a Docker environment
6. Running code in different environments
7. Other environments

## Before we start
We will learn how to set up different environments for computing, but we will assume that you currently have a Python interpreter installed in your computer.

In case you don't have any interpreter yet, we will learn how to install Python environments, so if you can't test/try a propoped action, go back to there when you learnt how to install one.

## Content
### What is a Data Processing Environment
We will take the following definition:

> A Data Processing Environment is a collection of software tools that enable the execution of computer programs for data transformation and maniupulation

We can highlight the following components as essential:
* Code intepreter or compiler
* Set of data processing libraries
* Text editor

Optionally we may want to use:
* Code debugger
* Integrated Developement Environmanet
* Source version control tools

### Code interpreter
During all the course, the Python programming language will be the prefered language. Mainly we will try to use version 3.6, however it is possible that in some cases, due to dependency contraints we may be using other versions.

We can use the Python interpreter in two flavours:
* **Run as script:** basically we will provide the script (i.e. path to the script) and its parameters as arguments. The results can be shown through the terminal or saved to a disk. 
  Some useful options of the python interpreter:
  * `-c`option: you can pass the code as argument
  * `-i`option: after running the code the intepreter remains interactive with the execution environment active and loaded. To quit the interpreter you can write `quit()` or type `Ctrl-D`. 

If you have a python interpreter installed try the following (otherwise just follow the Lab until we see how to install a new one and the come back here):

> Run the intepreter passing the code as argument 
```
$ python -c "print('Hello world')"
```

> Run the interpreter passing the code and remaining interactive after the execution
```
$ python -i -c "print('Hello world'); a = 0"
Hello world
>>> a
0
>>> 
```
* **Run interactivelly:** we can basically invoke the interpreter and then write the code interactivelly, this means that after each code block the interpreter will output the result and wait for the next code block.

```
$ python
>>> "Hello world"
'Hello world'
>>> a = 0
>>> a
0
>>> 
```

The main difference between the two running methods (in our concernings) is that each time we run a process as a script, we load data and we apply transformations from scratch each time.

If we run the code interactively, al data structures are loaded in memory and thus, we can transform the data from previous step without having to load again.

### Text editor and Integrated Developement Environments
We will not set preference in coding tools, however let's take a look at the different options:

* **Terminal text editor:** most of the Operating Systems have several editors that may be used in the terminal itself without the need of an external window. `vi`, `nano`, `emacs` are some examples of the most used editors. 
  * Pros: you can find them in almost any Unix-like operating system. They can be very useful if you want to make a small script or quick modifications. Essential if you do not have a graphical session. Really fast editing when you have a good knowledge.
  * Cons: not user friendly. They may require some learning before you can use it effecively. Hard to integrate in productive environments.

* **Graphical text editor:** these are raw text editors, where you can simply write code and save it. `gedit`, `notepad`, `textedit` are some examples. Normally all OS have a default text editor and they may be even cofigurable for programming or scripting. Beyond the default text editors we also can find a set of text editors that are specially developed for coding. 

For example:
  * Sublime Text (https://www.sublimehq.com)
  * Atom (https://atom.io/)
  * Visual Studio Code (https://code.visualstudio.com/)

These enchanced editors may include:
  * Syntax higlighting
  * Terminal and interpreter integration
  * Source control tools
  * Debugging tools
  * In general a lot of configurable settings

* **Integrated Developement Environment:** these programs include all the tools for the developement of a software project, ranging from code edition to deployment. They are quite big projects that led to heavy programs. Some examples of Python IDEs are:
  * PyCharm (https://www.jetbrains.com/pycharm/)
  * Eclipse (PyDev: https://www.pydev.org/)

* **Interactive Developement Tools:** we have seen how can we use the Python interpreter to interactively run code. This programming paradigm is very useful when a programmer is developing a Data Centric application. 

The main reason is that, as data is the most important asset, the programmer can run code to transform data, analyze, visualize, whilst maintaining the data into memory.

There are some tools that may help to interactively develop code and provide insights and results.

  * **IPython**: it is a big project that contain multiple tools for interactive computing. One of its main goals is to provide interactive shell superior to Python’s default. Quoting its docs: 
    > IPython has many features for tab-completion, object introspection, system shell access, command history retrieval across sessions, and its own special command system for adding functionality when working interactively. It tries to be a very efficient environment both for Python code development and for exploration of problems using Python objects (in situations like data analysis).

  * **Jupyter Notebooks:** IPython has a tool called Jupyter Notebooks or simply notebooks that allow the programert to run code in a web browser using a rich interpreter. This interpreter is based in "cells" taht can contain plain text or code. The Code cells can be run and the result is shown in the notebook.

  * **Spyder IDE:** another IPython feature is the kernel. Any IDE can connect to a IPython kernel so the editor code can be run in the kernel (i.e. IPython interopreter). Spyder mimics the layout of the popular programming environments RStudio and Matlab.

## Overview of the different configurations

Before we start the overview of the diferent proposal for creating a processing environment, let's try to understand the problem we will try to tackle.

 **What's wrong with my installed python version?** probably nothing. It's all right (I hope so).

 **So why do I need to setup different processing environments?** the problem are the libraries (modules and packages) you're going to need in the future and its dependencies.

 (*the following part is taken from the Python tutorial:* https://docs.python.org/3.6/tutorial/venv.html)

You may face several situations where you want to use different Python environments:
* An specific application needs a version different from the installed in your OS. Simply uninstaling the OS Python version is not a good idea as you may break many things.
* You want to use a module or a package that has a dependency of a specific version of another module or package, the requirements are in conflict.

There may be other situations where you may want to use different Python interpreters. The solution to this problem has been already added to the standard Python distribution.

Virtual environments, are self-contained directory trees that contain a Python installation for a particular version of Python, plus a number of additional packages.

Different applications can then use different virtual environments. To resolve the earlier example of conflicting requirements, application A can have its own virtual environment with version 1.0 installed while application B has another virtual environment with version 2.0. If application B requires a library be upgraded to version 3.0, this will not affect application A’s environment.

### How to create a virtual environment

As we said there's a module in the standard Python library called `venv`that will help us to create virtual environments (I will refer to virtual environment(s) as venv(s) ahead).

`venv` will usually install the most recent version of Python that you have available. 
 
If you have multiple versions of Python on your system, you can select a specific Python version by running python3 or whichever version you want.

To create a virtual environment, decide upon a directory where you want to place it, and run the venv module as a script with the directory path:

Normally is a good idea to create a directory named `.venv` and create venvs in there 

```
$ mkdir .venv
$ python3 -m venv tutorial-env
$ ls
tutorial-env
```

This will create the tutorial-env directory if it doesn’t exist, and also create directories inside it containing a copy of the Python interpreter, the standard library, and various supporting files.

```
$ ls tutorial-env/
bin             include         lib             pyvenv.cfg
```

Once we have the venv created we may want to run the python interpreted we've just created. To do so, we have to activate the environment first.

On Windows, run:

```
tutorial-env\Scripts\activate.bat
```

On Unix or MacOS, run:

```
$ source tutorial-env/bin/activate
```

Activating the virtual environment will change your shell’s prompt to show what virtual environment you’re using, and modify the environment so that running python will get you that particular version and installation of Python. For example:

```
$ source tutorial-env/bin/activate
(tutorial-env) $ python
Python 3.7.4 (default, Jul  9 2019, 16:32:37) 
[GCC 9.1.1 20190503 (Red Hat 9.1.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/usr/lib64/python37.zip', '/usr/lib64/python3.7', '/usr/lib64/python3.7/lib-dynload', '/root/.venv/tutorial-env/lib64/python3.7/site-packages', '/root/.venv/tutorial-env/lib/python3.7/site-packages']
>>>
```

Let's compare to the base python interpreter in our OS. 

Let's deactivate the environment first.

```
(tutorial-env) $ deactivate
$ python
Python 2.7.16 (default, Apr 30 2019, 15:54:43) 
[GCC 9.0.1 20190312 (Red Hat 9.0.1-0.10)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/usr/lib/python27.zip', '/usr/lib64/python2.7', '/usr/lib64/python2.7/plat-linux2', '/usr/lib64/python2.7/lib-tk', '/usr/lib64/python2.7/lib-old', '/usr/lib64/python2.7/lib-dynload', '/usr/lib64/python2.7/site-packages', '/usr/lib/python2.7/site-packages']
>>>
```

We can see some differences in the `PYTHONPATH`, which now is pointing to some of the directories `venv`created.
  
### Managing Packages with pip

There are several ways of adding modules and packages to a python interpreter (a.k.a install packages or modules). 

However, most of the time we would like to simply install a package or module from a repository. 

Pip is the preferred tool to install packages. By default pip will install packages from the Python Package Index, <https://pypi.org>. 

You can browse the Python Package Index by going to it in your web browser, or you can use pip’s limited search feature:

```
$ pip3 search data
data (0.4)                        - Work with unicode/non-unicode data from files or strings uniformly.
data-refinery (0.2.13)            - Data Refinery: transformating data
data-pack (0.0)                   - Data packages
data-analyze (1.4.1)              - data-analyze
.
.
.
```
#### Installing packages
You can install the latest version of a package by specifying a package’s name:

```
(tutorial-env) $ pip install pathlib
Collecting pathlib
  Downloading https://files.pythonhosted.org/packages/ac/aa/9b065a76b9af472437a0059f77e8f962fe350438b927cb80184c32f075eb/pathlib-1.0.1.tar.gz (49kB)
    100% |████████████████████████████████| 51kB 960kB/s 
Installing collected packages: pathlib
  Running setup.py install for pathlib ... done
Successfully installed pathlib-1.0.1
```

You can also install a specific version of a package by giving the package name followed by == and the version number:

```
(tutorial-env) $ pip install requests==2.6.0
Collecting requests==2.6.0
  Downloading https://files.pythonhosted.org/packages/73/63/b0729be549494a3e31316437053bc4e0a8bb71a07a6ee6059434b8f1cd5f/requests-2.6.0-py2.py3-none-any.whl (469kB)
    100% |████████████████████████████████| 471kB 3.1MB/s 
Installing collected packages: requests
Successfully installed requests-2.6.0
```

If you re-run this command, pip will notice that the requested version is already installed and do nothing. You can supply a different version number to get that version, or you can run pip install --upgrade to upgrade the package to the latest version:

```
(tutorial-env) $ pip install --upgrade requests
Collecting requests
  Downloading https://files.pythonhosted.org/packages/51/bd/23c926cd341ea6b7dd0b2a00aba99ae0f828be89d72b2190f27c11d4b7fb/requests-2.22.0-py2.py3-none-any.whl (57kB)
    100% |████████████████████████████████| 61kB 1.2MB/s 
Collecting chardet<3.1.0,>=3.0.2 (from requests)
  Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)
    100% |████████████████████████████████| 143kB 2.7MB/s 
Collecting idna<2.9,>=2.5 (from requests)
  Downloading https://files.pythonhosted.org/packages/14/2c/cd551d81dbe15200be1cf41cd03869a46fe7226e7450af7a6545bfc474c9/idna-2.8-py2.py3-none-any.whl (58kB)
    100% |████████████████████████████████| 61kB 5.1MB/s 
Collecting urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 (from requests)
  Downloading https://files.pythonhosted.org/packages/e0/da/55f51ea951e1b7c63a579c09dd7db825bb730ec1fe9c0180fc77bfb31448/urllib3-1.25.6-py2.py3-none-any.whl (125kB)
    100% |████████████████████████████████| 133kB 2.8MB/s 
Collecting certifi>=2017.4.17 (from requests)
  Downloading https://files.pythonhosted.org/packages/18/b0/8146a4f8dd402f60744fa380bc73ca47303cccf8b9190fd16a827281eac2/certifi-2019.9.11-py2.py3-none-any.whl (154kB)
    100% |████████████████████████████████| 163kB 2.4MB/s 
Installing collected packages: chardet, idna, urllib3, certifi, requests
  Found existing installation: requests 2.6.0
    Uninstalling requests-2.6.0:
      Successfully uninstalled requests-2.6.0
Successfully installed certifi-2019.9.11 chardet-3.0.4 idna-2.8 requests-2.22.0 urllib3-1.25.6
```

Note that pip will also resolve package dependencies, download and install them.

pip uninstall followed by one or more package names will remove the packages from the virtual environment.

#### Managing installed packages and modules
pip show will display information about a particular package:

```
(tutorial-env) $ pip show requests
Name: requests
Version: 2.22.0
Summary: Python HTTP for Humans.
Home-page: http://python-requests.org
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: /usr/lib/python2.7/site-packages
Requires: chardet, idna, urllib3, certifi
Required-by: 
```

pip list will display all of the packages installed in the virtual environment:

```
(tutorial-env) $ pip list
Package         Version 
--------------- --------
cson            0.7     
dbus-python     1.2.8   
gpg             1.12.0  
numpy           1.16.4  
olefile         0.46    
pandas          0.25.2  
pathlib         1.0.1   
Pillow          5.4.1   
pip             19.0.3  
Pygments        2.2.0   
PyQt5           5.13.1  
PyQt5-sip       4.19.19 
python-dateutil 2.8.0   
pytz            2019.3  
rpm             4.14.2.1
selinux         2.9     
sepolicy        1.1     
setools         4.1.1   
setuptools      40.8.0  
six             1.12.0  
speg            0.3     
```

pip freeze will produce a similar list of the installed packages, but the output uses the format that pip install expects. A common convention is to put this list in a requirements.txt file:

```
(tutorial-env) $ pip freeze > requirements.txt
(tutorial-env) $ cat requirements.txt
cson==0.7
dbus-python==1.2.8
gpg==1.12.0
numpy==1.16.4
olefile==0.46
pandas==0.25.2
pathlib==1.0.1
Pillow==5.4.1
Pygments==2.2.0
PyQt5==5.13.1
PyQt5-sip==4.19.19
python-dateutil==2.8.0
pytz==2019.3
rpm==4.14.2.1
selinux==2.9
sepolicy==1.1
setools==4.1.1
six==1.12.0
speg==0.3
```

The requirements.txt can then be committed to version control and shipped as part of an application. 

Users can then install all the necessary packages with install -r:

```
(tutorial-env) $ python3 -m venv tutorial-env2
(tutorial-env) $ source tutorial-env2/bin/activate
(tutorial-env2) $ pip install -r requirements.txt
Collecting certifi==2019.9.11 (from -r requirements.txt (line 1))
  Using cached https://files.pythonhosted.org/packages/18/b0/8146a4f8dd402f60744fa380bc73ca47303cccf8b9190fd16a827281eac2/certifi-2019.9.11-py2.py3-none-any.whl
...
Collecting urllib3==1.25.6 (from -r requirements.txt (line 5))
  Using cached https://files.pythonhosted.org/packages/e0/da/55f51ea951e1b7c63a579c09dd7db825bb730ec1fe9c0180fc77bfb31448/urllib3-1.25.6-py2.py3-none-any.whl
Installing collected packages: certifi, chardet, idna, urllib3, requests
Successfully installed certifi-2019.9.11 chardet-3.0.4 idna-2.8 requests-2.22.0 urllib3-1.25.6
```

pip has many more options. Consult the Installing Python Modules guide for complete documentation for pip. 


## Conclusions

## References

* Ipython: https://ipython.readthedocs.io/
* The Python Tutorial: https://docs.python.org/3.6/tutorial
* Python virtualenv tutorial: https://docs.python.org/3/tutorial/venv.html#virtual-environments-and-packages
* Python Modules guide: https://docs.python.org/3/installing/index.html#installing-index