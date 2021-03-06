## Requirements

* Python 3.6 or higher
* The following Python libraries:
    * [ruamel.yaml](https://yaml.readthedocs.io/en/latest/index.html)
    * [Requests](https://2.python-requests.org/en/master/)
    * [progress_bar](https://pypi.org/project/progress_bar/)
    * [openpyxl](https://pypi.org/project/openpyxl/)
    * If you want to have these libraries automatically installed, you will need Python's [setuptools](https://pypi.org/project/setuptools/)
* An [Islandora](https://islandora.ca/) repository with the [Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module enabled.

Islandora Workbench has been installed and used on Linux, Mac, and Windows.

!!! warning
    Some systems have both Python 2 and Python 3 installed. It's a good idea to check which version is used when you run `python`. To do this, run `python --version`, which will output something like "Python 2.7.17" or "Python 3.6.9". If `python --version` indicates you're running version 2, try running `python3 --version` to see if you have version 3 installed.

## Installing Islandora Workbench

If you already have the required Python libraries installed, simply cloning Islandora Workbench from GitHub is all you need to do:

`git clone https://github.com/mjordan/islandora_workbench.git`

This will create a directory named `islandora_workbench` where you will run the `./workbench` command.

If you don't already have the required libraries installed, clone this repo as above, and then use `setup.py`:

`sudo python3 setup.py install`

After you run this, all of the required Python libraries will be installed.

## Updating Islandora Workbench

Since Islandora Workbench is under development, you will want to update it often. To do this, run the following `git` command:

`git pull origin main`

After you pull in the latest changes using `git`, it's a good idea to rerun the setup tools in case new Python libraries have been added since you last ran the setup tools (same command as above):

`sudo python3 setup.py install`
