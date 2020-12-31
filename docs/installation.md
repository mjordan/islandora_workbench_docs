## Requirements

* Python 3.2 or higher
    * The [ruamel.yaml](https://yaml.readthedocs.io/en/latest/index.html) library
    * The [Requests](https://2.python-requests.org/en/master/) library
    * Python's [setuptools](https://pypi.org/project/setuptools/)
* An [Islandora](https://islandora.ca/) repository
* The [Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module.

## Installation

If you already have ruamel.yaml and requests installed, simply cloning Islandora Workbench from GitHub is all you need to do:

* `git clone https://github.com/mjordan/islandora_workbench.git`

If you don't already have the two required libraries installed, clone this repo as above, and then use `setup.py`:

* `sudo python3 setup.py install`

