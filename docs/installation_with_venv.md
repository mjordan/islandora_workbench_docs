## Installing Workbench inside of a Python virtual environment

Within the Python community it is strongly recommended that software is installed within what are called Python "virtual environments." Python virtual environments are an isolated way to install the required Python modules of an application that avoid conflicts with other Python software and modules. Essentially they are a special directory that will contain the required Python modules for Workbench and a few other files. They are not related to virtual machines (VMs) or Docker containers, though they can be used in conjunction with VMs and Docker.

! note
    These instructions assume you have already installed `git` and a version of Python that is compatible with Workbench.

### Overview

#### The high level steps for INSTALLING Workbench inside a Python virtual environments:

The following steps are normally only run once.

(Open a terminal window)

1. Clone Workbench git repository
2. Inside of git repo folder, create a virtual environment folder
3. Activate the virtual environment in your current terminal window
4. Inside that terminal window install the Workbench required modules

#### The high level steps for USING Python virtual environments when running Workbench:

These steps have to be run each time you want to use Islandora Workbench.

(Open a terminal window)

1. Inside of the Workbench git repo folder, activate the virtual environment in your current terminal window
2. run the `workbench` commands inside that window

### Step 1: Clone Workbench git repository

(Open a terminal window)

cd to a directory that you want to use for installing Workbench

As an example we will use `cd /Users/jdoe/Documents/`

Clone the Islandora Workbench git repo inside of the path you chose

`git clone https://github.com/mjordan/islandora_workbench.git`

This will create a folder called "islandora\_workbench" at this path:

`/Users/jdoe/Documents/islandora_workbench`

### Step 2: Create a Python virtual environment

cd into the newly created folder called islandora\_workbench that contains the git repo from the section above.

`cd islandora_workbench`

Use the `venv` module create a Python "virtual environment" called "venv"
`python3 -m venv venv`

### Step 3: Activate your virtual environment

In your terminal window you must FIRST activate the environment every time you want to **install**, **use**, or **upgrade** Islandora Workbench.

For activating the virtual environment you need to use the environment folder you created above, which was called "venv". First make sure you are inside the directory that has the Workbench repository. In this example it would be:

`cd /Users/jdoe/Documents/islandora_workbench`

Then activate the environment with this command (Linux/macOS):
`source venv/bin/activate`

Windows activate syntax:
`venv\Scripts\activate`

TIP: You can verify that the environment is activated by noticing a change in the left most prompt in our terminal window, the name of the environment folder will show up on the far left inside parentheses: "(venv)"

Before activation example:
`jdoe-laptop:islandora_workbench_fork jdoe$`

After activation example:
`(venv) jdoe-laptop::islandora_workbench_fork jdoe$`

TIP: Another way to tell you are running a virtual environment is to run this command to print the full path on your virtual environment. If the command prints and **empty** line, your virtual environment is NOT active

macOS:
`echo $VIRTUAL_ENV`

Windows:
`echo %VIRTUAL_ENV%`

### Step 4: Install Workbench's required Python modules inside a virtual environment

IMPORTANT/STOP: You need to make sure the environment is activated at this point, see activation instructions above:

After activation example:
`(venv) jdoe-laptop::islandora_workbench_fork jdoe$`

Run this command to start the process to install the required Python modules

NOTE: There single period below to the right of the word "install"

`python3 -m pip install .`

Congrats, if there are no errors, at this point Islandora Workbench has been installed inside the virtual environment.

!!! note
    If you get an error similar to `ModuleNotFoundError: No module named 'setuptools` which can happen if you are running Python version 3.12 or newer, you may need to run `python3 -m pip install setuptools` before step 4 above.


---

## Everyday workflow to use Workbench with an existing virtual a environment

If you followed the instructions above for creating the virtual environment, this would be the instructions to regularly use Islandora Workbench with a virtual environment.

### Step 1: Activate virtual environment

cd into the folder called islandora\_workbench that contains the git repo from the section above
`cd /Users/jdoe/Documents/islandora_workbench`

Then activate the environment with this command (Unix/macOS):
`source venv/bin/activate`

Windows activate syntax:
`venv\Scripts\activate`

IMPORTANT/STOP: You need to make sure the environment is activated at this point, see activation instructions above:

After activation example:
`(venv) jdoe-laptop::islandora_workbench_fork jdoe$`

TIP: Another way to tell you are running a virtual environment is to run this command to print the full path on your virtual environment. If the command prints and **empty** line, your virtual environment is NOT active

macOS:
`echo $VIRTUAL_ENV`

Windows:
`echo %VIRTUAL_ENV%`

### Step 2: Run Workbench commands

Track down the paths to your `config.yml` and `metadata.csv` files to then be used when you run the `workbench` command on your terminal window.

`./workbench --config path/to/config.yml --check`

---

## Updating Islandora Workbench within a virtual environment

Since Islandora Workbench is under development, you will want to update it often.

### Step 1: Activate virtual environment

To do this, within the islandora\_workbench directory, cd into the folder called islandora\_workbench that contains the git repo from the section above
`cd /Users/jdoe/Documents/islandora_workbench`

### Step 2: Pull in latest Workbench changes from repo

run the following git command:
`git pull origin main`


### Step 2: Activate virtual environment

Then activate the environment with this command (Unix/macOS):
`source venv/bin/activate`

Windows activate syntax:
`venv\Scripts\activate`

IMPORTANT/STOP: You need to make sure the environment is activated at this point, see activation instructions above:

After activation example:
`(venv) jdoe-laptop::islandora_Workbench_fork jdoe$`

TIP: Another way to tell you are running a virtual environment is to run this command to print the full path on your virtual environment. If the command prints and **empty** line, your virtual environment is NOT active

macOS:
`echo $VIRTUAL_ENV`

Windows:
`echo %VIRTUAL_ENV%`

### Step 3: Upgrade Workbench's required Python modules inside a virtual environment

Since you have already pulled in the latest changes using git, see above, it's a good idea to rerun the setup tools in case new Python libraries have been added since you last ran the "pip install":
`sudo python -m pip install --upgrade .`

NOTE: There single period below to the right of the word "--upgrade"

---

## Condensed instructions



### Condensed: Installing Workbench inside of a Python virtual environment

You only need to run these steps once.

`cd /Users/jdoe/Documents/`

`git clone https://github.com/mjordan/islandora_workbench.git`

`cd islandora_workbench`

`python3 -m venv venv`

`source venv/bin/activate`

Windows activate syntax:
`venv\Scripts\activate`

STOP: Make sure environment was activated

After activation example:
`(venv) jdoe-laptop::islandora_workbench_fork jdoe$`

TIP: Another way to tell you are running a virtual environment is to run this command to print the full path on your virtual environment. If the command prints and **empty** line, your virtual environment is NOT active

Linux/macOS:
`echo $VIRTUAL_ENV`

Windows:
`echo %VIRTUAL_ENV%`

NOTE: there is a period to the right of "install"
`python3 -m pip install .`

### Condensed: Everyday workflow to use Workbench with an existing virtual a environment

You need to run these steps whenever you open a brand new terminal window to run `workbench` commands.

`cd /Users/jdoe/Documents/islandora_workbench`

`source venv/bin/activate`

Windows activate syntax:
`venv\Scripts\activate`

STOP: Make sure environment was activated

After activation example:
`(venv) jdoe-laptop::islandora_workbench jdoe$`

TIP: Another way to tell you are running a virtual environment is to run this command to print the full path on your virtual environment. If the command prints an **empty** line, your virtual environment is NOT active.

macOS:
`echo $VIRTUAL_ENV`

Windows:
`echo %VIRTUAL_ENV%`

Create your config.yml and metadata.csv files.

`./workbench --config path/to/config.yml --check`

## Common errors

### Error if the virtual environment is not activated (after installation inside environment)

This is the error you may get if you had previously installed Workbench in a virtual environment but forgot to activate it:

`./workbench --config path/to/config.yml --check`
`Traceback (most recent call last):`
  `File "/Users/ysuarez/Documents/Archives/islandora_workbench_local_berklee_use/./workbench", line 16, in <module>`
    `import requests_cache`
`ModuleNotFoundError: No module named 'requests_cache'`

That is because normally the "base" Python installation on your computer would not have all of the modules that you should have previously installed in your virtual environment.

#### Solution:

* Activate the virtual environment, see above.
* Then re-run your `workbench` command.

### ModuleNotFoundError: No module named 'setuptools'

This error may arise from using Python 3.12 or newer, which now requires you to install  the `setuptools` package manually inside your venvs.

#### Solution:

You will need to run this command….

`python3 -m pip install setuptools`