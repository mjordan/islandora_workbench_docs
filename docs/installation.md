## Requirements

* Python 3.2 or higher
* The following Python libraries:
    * [ruamel.yaml](https://yaml.readthedocs.io/en/latest/index.html)
    * [Requests](https://2.python-requests.org/en/master/)
    * [progress_bar](https://pypi.org/project/progress_bar/)
    * [openpyxl](https://pypi.org/project/openpyxl/)
    * If you want to have these libraries automatically installed, you will need Python's [setuptools](https://pypi.org/project/setuptools/)
* An [Islandora](https://islandora.ca/) repository with the [Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module enabled.

Islandora Workbench has been installed and used on Linux, Mac, and Windows.

## Installation

If you already have the required Python libraries installed, simply cloning Islandora Workbench from GitHub is all you need to do:

* `git clone https://github.com/mjordan/islandora_workbench.git`

This will create a directory named `islandora_workbench` where you will run the `./workbench` command.

If you don't already have the required libraries installed, clone this repo as above, and then use `setup.py`:

* `sudo python3 setup.py install`

After you run this, all of the required Python libraries will be installed.
