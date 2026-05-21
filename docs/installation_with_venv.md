## Installing Workbench inside of a Python virtual environment

Current best practice for Python application is to install them within what are called Python "virtual environments." Python virtual environments isolate an application's Python modules in a way that avoids conflicts with other Python software and modules on your computer. Essentially a virtual environment is a special directory that contains the required Python modules for the application and a few other files. Python virtual environments are not related to virtual machines (VMs) or Docker containers, though they can be used in conjunction with VMs and Docker.

!!! note
    These instructions assume you have already installed `git` and a version of Python that is compatible with Workbench (currently 3.11 or higher).

### Overview

!!! note
    Below you will see the command `python3 -m venv venv` in several places. The two instances of "venv" have distinct meanings. The first is the name of a a Python module and the second is the name of the virtual environment directory running the command will create. Using "venv" for the directory name looks confusing, but it is a convention within the Python community that is widely used.

!!! note
    The documentation below uses terminal command prompt examples from macOS, e.g. `jdoe-laptop:islandora_workbench jdoe$` and `(venv) jdoe-laptop::islandora_workbench jdoe$`. Command prompts on your Linux or Windows computer will look a little different:

    Linux: `jdoe@computer-name:` and `(venv) jdoe@computer-name:`

    Windows: `C:\Users\jdoe` and `(venv) C:\Users\jdoe`

#### The high level steps for *installing* Workbench inside a Python virtual environments:

The following steps are normally only run once.

(Open a terminal window)

1. Clone the Workbench git repository
2. Inside of the resulting `islandora_workbench` directory, create a virtual environment
3. Activate the virtual environment in your current terminal window
4. Inside that terminal window install the Workbench required modules

#### The high level steps for *using* Python virtual environments when running Workbench:

These steps have to be run each time you open a new terminal window to use Islandora Workbench.

(Open a terminal window)

1. Inside of the Workbench git repo folder, activate the virtual environment in your current terminal window
2. run the `workbench` commands inside that window

### Step 1: Clone Workbench git repository

(Open a terminal window)

`cd` to the directory where you want to install Workbench. As an example we will use `/Users/jdoe/Documents/` as the parent directory for the `islandora_workbench` directory.

`cd /Users/jdoe/Documents/`

Clone the Islandora Workbench git repo inside of the path you chose:

`git clone https://github.com/mjordan/islandora_workbench.git`

This will create a folder called "islandora\_workbench" at this path:

`/Users/jdoe/Documents/islandora_workbench`

### Step 2: Create a Python virtual environment

`cd` into the newly created folder called "islandora\_workbench" that contains the cloned git repo.

`cd islandora_workbench`

Use the `venv` module create a Python "virtual environment" called "venv" by typing `python3 -m venv venv`

### Step 3: Activate your virtual environment

You must activate the virtual environment every time you open a new terminal window to *install*, *use*, or *upgrade* Islandora Workbench.

To activate the virtual environment you need to run a command referencing the environment folder you created above, which was called "venv". First make sure you are inside the directory that has the Workbench repository. In this example it would be:

`cd /Users/jdoe/Documents/islandora_workbench`

Then activate the environment with this command:

* Linux/macOS: `source venv/bin/activate`
* Windows: `venv\Scripts\activate`

!!! note
    You can verify that the environment is active by looking for a the presence of "(venv)" at the beginning of the command prompt. This is the name of the evirtual nvironment folder you created by runnig `python3 -m venv venv`:

    Before activation example: `jdoe-laptop:islandora_workbench jdoe$`

    After activation example: `(venv) jdoe-laptop::islandora_workbench jdoe$`

### Step 4: Install Workbench's required Python modules inside your virtual environment

*Important/stop*: You need to make sure the environment is activate at this point; if it is not, refer to the activation instructions above:

After activation, you will see "(venv)" at the beginning of your terminal's command prompt: `(venv) jdoe-laptop::islandora_workbench jdoe$`

Run this command to start the process to install the required Python modules (note there single period to the right of the word "install")

`python3 -m pip install .`

Congratulations, if there are no errors, at this point Islandora Workbench has been installed inside the virtual environment.

!!! note
    If you get an error similar to `ModuleNotFoundError: No module named 'setuptools'` which can happen if you are running Python version 3.12 or newer, you may need to run `python3 -m pip install setuptools` before step 4 above.


## Everyday workflow for using Workbench within an existing virtual environment

Now that you have created your virtual environment for Workbench, the following are the instructions for daily use of Islandora Workbench within that virtual environment.

### Step 1: Activate the virtual environment

`cd` into the folder called islandora\_workbench that contains the git repo from the section above by running the following command: `cd /Users/jdoe/Documents/islandora_workbench`

Then activate the environment with this command:

* Linux/macOS: `source venv/bin/activate`
* Windows: `venv\Scripts\activate`

*Important/stop*: You need to make sure the environment is active at this point, see activation instructions above:

After activation, you will see "(venv)" at the beginning of your terminal's command prompt: `(venv) jdoe-laptop::islandora_workbench jdoe$`

### Step 2: Run Workbench commands

Within your virtual environment, you can now run Workbench:

`./workbench --config config.yml --check`


## Updating Islandora Workbench within a existing virtual environment

Since Islandora Workbench needs to be updated frequently, it is important to keep your copy up to date.

### Step 1: Change into the virtual environment directory

`cd` into the folder called "islandora\_workbench" that contains the git repo from the section above: `cd /Users/jdoe/Documents/islandora_workbench`

### Step 2: Pull in latest Workbench changes from Github

Run the following git command: `git pull origin main`


### Step 3: Activate virtual environment

Then activate the environment with this command:

* Linux/macOS: `source venv/bin/activate`
* Windows: `venv\Scripts\activate`

*Important/stop*: You need to make sure the environment is activated at this point, see activation instructions above:

After activation, you will see "(venv)" at the beginning of your terminal's command prompt: `(venv) jdoe-laptop::islandora_workbench jdoe$`

### Step 4: Upgrade Workbench's required Python modules inside a virtual environment

Since you have already pulled in the latest changes using git, see above, it's a good idea to rerun the setup tools in case new Python libraries have been added since you last ran the "pip install". Within your active virtual environment directory, run:

`python -m pip install --upgrade .`

Note: There single period to the right of the word "--upgrade"

## Condensed instructions

### Condensed: Installing Workbench inside of a Python virtual environment

You only need to run these steps once.

`cd /Users/jdoe/Documents/`

`git clone https://github.com/mjordan/islandora_workbench.git`

`cd islandora_workbench`

`python3 -m venv venv`

* Linux/macOS: `source venv/bin/activate`
* Windows: `venv\Scripts\activate`

*Important/stop*: Make sure environment was activated.

After activation, you should see "(venv)" at the start of your prompt: `(venv) jdoe-laptop::islandora_workbench jdoe$`

Note: there is a period to the right of "install": `python3 -m pip install .`

### Condensed: Everyday workflow to use Workbench within an existing virtual a environment

You need to run these steps whenever you open a new terminal window to run Workbench.

1. `cd /Users/jdoe/Documents/islandora_workbench`
2. activate your virtual environment:
    - Linux/macOS: `source venv/bin/activate`
    - Windows: `venv\Scripts\activate`

*Important/stop*: Make sure the virtual environment was activated.

After activation, you should see "(venv)" at the start of your prompt: `(venv) jdoe-laptop::islandora_workbench jdoe$`

You are now ready to run Workbench: `./workbench --config path/to/config.yml --check`

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

* Activate the virtual environment as described above.
* Then re-run your `workbench` command.

### ModuleNotFoundError: No module named 'setuptools'

This error may arise from using Python 3.12 or newer, which now requires you to install  the `setuptools` package manually inside your venvs.

#### Solution:

You will need to run this command: `python3 -m pip install setuptools`