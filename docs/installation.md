## Requirements

* An modern [Islandora](https://islandora.ca/) repository, with the [Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module enabled.
* Python 3.9 or higher
* The following Python libraries:
    * [ruamel.yaml](https://yaml.readthedocs.io/en/latest/index.html)
    * [Requests](https://2.python-requests.org/en/master/)
    * [Requests-Cache](https://pypi.org/project/requests-cache/)
    * [progress_bar](https://pypi.org/project/progress_bar/)
    * [openpyxl](https://pypi.org/project/openpyxl/)
    * [unidecode](https://pypi.org/project/Unidecode/)
    * [edtf-validate](https://pypi.org/project/edtf-validate/)
    * [rich](https://pypi.org/project/rich/)
    * If you want to have these libraries automatically installed, you will need [pip](https://pypi.org/project/pip/) version 19 or higher.

Islandora Workbench has been installed and used on Linux, Mac, and Windows.

!!! warning
    Some systems have both Python 2 and Python 3 installed. It's a good idea to check which version is used when you run `python`. To do this, run `python --version`, which will output something like "Python 2.7.17" or "Python 3.8.10". If `python --version` indicates you're running version 2, try running `python3 --version` to see if you have version 3 installed.

    Also, if you installed an alternate version of Python 3.x on your system (for example via Homebrew on a Mac), you may need to run Workbench by calling that Python interpreter directly. For Python 3.x installed via Homebrew, that will be at `/opt/homebrew/bin/python3`, so to run Workbench you would use `/opt/homebrew/bin/python3 workbench` while in the `islandora_workbench` directory.

If you are using Drupal 8.5 or earlier, please refer to the "Using Drupal 8.5 or earlier" section below. If you are using Python 3.8, please refer to that section below.

### macOS, Homebrew and pip

As of around 2024, trying to install Islandora Workbench on a Mac with Homebrew using pip will print a warning message and fail.

The preferred method for installing software via pip is now to use a virtual environment.

See [this StackOverflow answer](https://stackoverflow.com/a/78297842) for more information and steps to create a virtual environment where Islandora Workbench's dependencies can be installed.

## Installing Islandora Workbench

Installation involves two steps:

1. cloning the Islandora Workbench Github repo
1. running `python -m pip install .` to install the required Python libraries (listed above)

### Step 1: cloning the Islandora Workbench Github repo

In a terminal, run:

`git clone https://github.com/mjordan/islandora_workbench.git`

This will create a directory named `islandora_workbench` where you will run the `./workbench` command.

### Step 2: running setup.py to install the required Python libraries

For most people, the preferred place to install Python libraries is in your user directory. To do this, change into the "islandora_workbench" directory created by cloning the repo, and run the following command:

`python -m pip install --user .`

A less common method is to install the required Python libraries into your computer's central Python environment. To do this, omit the `--user` (note: you must have administrator privileges on the computer to do this):

`sudo python -m pip install .`

`sudo` is only required on Linux and Macs, on Windows you will need to run `python -m pip install .` as administrator.

## Updating Islandora Workbench

Since Islandora Workbench is under development, you will want to update it often. To do this, within the `islandora_workbench` directory, run the following `git` command:

`git pull origin main`

After you pull in the latest changes using `git`, it's a good idea to rerun the setup tools in case new Python libraries have been added since you last ran the setup tools (same command as above):

`python -m pip install --user --upgrade .`

or if you originally installed the required Python libraries centrally, without the `--user` option (again, you will need administrator privileges on the machine):

`sudo python -m pip install --upgrade .`

## Keeping the Islandora Workbench Integration Drupal module up to date

Islandora Workbench communicates with Drupal using REST endpoints and Views. The Islandora Workbench Integration module (linked above in the "Requirements" section) ensures that the target Drupal has all required REST endpoints and Views enabled. Therefore, keeping it in sync with Islandora Workbench is important.

Workbench checks the version of the Integration module and tells you if you need to upgrade it. To upgrade the module, update its code via Git or Composer, and follow the instructions in the "Updates" section of its [README](https://github.com/mjordan/islandora_workbench_integration/blob/main/README.md).

## Configuring Drupal's media URLs

Islandora Workbench uses Drupal's default form of media URLs. You should not need to do anything to allow this, since the admin setting in `admin/config/media/media-settings` (under "Security") that determines what form of media URLs your site uses defaults to the correct setting (unchecked):

![Field machine names](images/standalone_media_url_setting.png)

If your site needs to have this option checked (so it supports URLs like `/media/{id}`), you will need to add the following entry to all configuration files for tasks that create or delete media:

`standalone_media_url: true`

!!! note
    If you change the checkbox in Drupal's media settings admin page, be sure you clear your Drupal cache to make the new media URLs work.

## Using Drupal 8.5 or earlier

When ingesting media in Drupal versions 8.5 and earlier, Islandora Workbench has two significant limitations/bugs that you should be aware of:

* Approximately 10% of media creation attempts will likely fail. Workbench will log these failures. Additional information is available in this [issue](https://github.com/Islandora/documentation/issues/1481).
* A file with a filename that already exists in Islandora will overwrite the existing file, as reported in this [issue](https://github.com/Islandora/documentation/issues/1790).

To avoid these issues, you need to be running Drupal version 8.6 or higher.

!!! warning
    If you are using Drupal 8.5 or earlier, you need to use the version of Workbench tagged with `drupal_8.5_and_lower` (commit 542325fb6d44c2ac84a4e2965289bb9f9ed9bf68). Later versions no longer support Drupal 8.5 and earlier.

## Using Python 3.8

Python 3.8 reached end of life in October 2024. If you cannot upgrade from Python 3.8, you may be interested in the [Python 3.8 EOL release](https://github.com/mjordan/islandora_workbench/releases/tag/python38eol) of Islandora Workbbench.


## Using Workbench within a Docker container

If you would rather avoid installing the Python libraries required by Islandora Workbench on your computer, and you have `make` and Docker installed, you can build a container and run Workbench within it easily. To do so, follow these steps:

1. Clone Islandora Workbench from Github as described above.
1. In the `islandora_workbench` home directory, run `make run-workbench-in-docker`. This builds the container, mounts the `islandora_workbench` directory inside the container at `/workbench`, and puts you into a shell within that directory.
1. To exit the container and shut it down, type `exit`, which will place you back into your `islandora_workbench` home directory.

If you want to rebuild the container using the most recent Workbench code, run the necessary `git` commands as documented in the "Updating Islandora Workbench" section above and then run `make rebuild-islandora-workbench`. The resulting rebuilt container will contain the most recent Islandora Workbench code.

!!! note
    The container maps the `islandora_workbench` directory on your host computer to the `/workbench` directory within the container. This means that you can configure and run Workbench in the same way you would as if it were installed on your host computer as long as all of your configuration files and input data are relative to, or absolute in relation to, your host computer's `islandora_workbencn` directory.

    For example, you can open a configuration file at `islandora_workbench/myconfig.yml` in your desktop text editor and modify it, but run Workbench within the container's `/workbench` directory like `python workbench --config myconfig.yml --check`. Same goes for values in your configuration's `input_dir` setting -- a value like `batch_01` (which is located at `islandora_workbench/batch_01` on your host computer) will be understood by Workbench running within the container as `/workbench/batch_01`.