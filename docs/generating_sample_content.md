If you want to quickly generate some sample images to load into Islandora, Workbench provides a utility script to do that.

Running `python3 scripts/generate_image_files.py` from within the Islandora Workbench directory will generate PNG images from the list of titles in the `sample_filenames.txt` file. Running this script will result in a group of images whose filenames are normalized versions of the lines in the sample title file. You can then load this sample content into Islandora using the `create_from_files` task.

If you want to have Workbench generate the sample content automatically, configure the `generate_image_files.py` script as a bootstrap script. See the `autogen_content.yml` configuration file for an example of how to do that.
