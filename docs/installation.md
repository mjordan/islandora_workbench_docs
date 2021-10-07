## Requirements

* An [Islandora](https://islandora.ca/) repository using Drupal 8 or 9, with the [Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module enabled. If you are using Drupal 8.5 or earlier, please refer to the "Using Drupal 8.5 or earlier" section below.
* Python 3.6 or higher
* The following Python libraries:
    * [ruamel.yaml](https://yaml.readthedocs.io/en/latest/index.html)
    * [Requests](https://2.python-requests.org/en/master/)
    * [progress_bar](https://pypi.org/project/progress_bar/)
    * [openpyxl](https://pypi.org/project/openpyxl/)
    * [unidecode](https://pypi.org/project/Unidecode/)
    * [edtf-validate](https://pypi.org/project/edtf-validate/)
    * If you want to have these libraries automatically installed, you will need Python's [setuptools](https://pypi.org/project/setuptools/)

Islandora Workbench has been installed and used on Linux, Mac, and Windows.

!!! warning
    Some systems have both Python 2 and Python 3 installed. It's a good idea to check which version is used when you run `python`. To do this, run `python --version`, which will output something like "Python 2.7.17" or "Python 3.6.9". If `python --version` indicates you're running version 2, try running `python3 --version` to see if you have version 3 installed.

## Installing Islandora Workbench

If you already have the required Python libraries installed, simply cloning Islandora Workbench from GitHub is all you need to do:

`git clone https://github.com/mjordan/islandora_workbench.git`

This will create a directory named `islandora_workbench` where you will run the `./workbench` command.

If you don't already have the required libraries installed, clone this repo as above, and then use `setup.py`:

`sudo python3 setup.py install`

If you would rather not install third-party libraries in your operating system's central Python location, or you do not have `sudo` priviliges on your computer, you can install them in your user directory:

`python3 setup.py install --user`

After you run either of these install commands, all of the required Python libraries will be installed.

## Updating Islandora Workbench

Since Islandora Workbench is under development, you will want to update it often. To do this, run the following `git` command:

`git pull origin main`

After you pull in the latest changes using `git`, it's a good idea to rerun the setup tools in case new Python libraries have been added since you last ran the setup tools (same command as above):

`sudo python3 setup.py install`

or if you originally installed Islandora Workbench using the `--user` option:

`python3 setup.py install --user`

## Keeping the Islandora Workbench Integration Drupal module up to date

Islandora Workbench communicates with Drupal using REST endpoints and Views. The Islandora Workbench Integration module (linked above in the "Requirements" section) ensures that the target Drupal has all required REST endpoints and Views enabled. Therefore, keeping it in sync with Islandora Workbench is important.

Announcments indicating that updating the Drupal module will be posted in the Islandora Slack, but if you want to be notified that there has been a change in Islandora Workbench that requires a corresponding change in the Integration module, follow the instructions in the "Post-merge hook script" section of the Islandora Workbench [README](https://github.com/mjordan/islandora_workbench).

## Using Drupal 8.5 or earlier

When ingesting media in Drupal versions 8.5 and earlier, Islandora Workbench has two significant limitations/bugs that you should be aware of:

* Approximately 10% of media creation attempts will likely fail. Workbench will log these failures. Additional information is available in this [issue](https://github.com/Islandora/documentation/issues/1481).
* A file with a filename that already exists in Islandora will overwrite the existing file, as reported in this [issue](https://github.com/Islandora/documentation/issues/1790).

To avoid these issues, you need to be running Drupal version 8.6 or higher.
