## Cancelling Workbench

You can cancel/quit Islandora Workbench manually, before it completes running. You normally wouldn't do this, but if you do *need* to cancel/quit a Workbench job, press `ctrl-c` (the standard way to exit a running script) while Workbench is running.

To illustrate what happens when you do this, let's use the following simple CSV input file, which we'll use to `create` some nodes:

```text
file,id,title
IMG_1410.tif,01,Small boats in Havana Harbour
IMG_2549.jp2,02,Manhatten Island
IMG_2940.JPG,03,Looking across Burrard Inlet
IMG_2958.JPG,04,Amsterdam waterfront
IMG_5083.JPG,05,Alcatraz Island
```

We run Workbench, and after two nodes have been created, we issue a `ctrl-c` to cancel:

```text
OK, connection to Drupal at http://localhost verified.
Node for "Small boats in Havana Harbour" (record 01) created at http://localhost/node/33.
Node for "Manhatten Island" (record 02) created at http://localhost/node/34.
^CExiting before entire CSV processed. See log for more info.
```

`workbench.log` will contain the following entries:

```text
07-Nov-21 09:39:30 - INFO - OK, connection to Drupal at http://localhost verified.
07-Nov-21 09:39:31 - INFO - Writing rollback CSV to input_data/rollback.csv
07-Nov-21 09:39:31 - INFO - "Create" task started using config file foo.yml
07-Nov-21 09:39:39 - INFO - "nodes_only" option in effect. No media will be created.
07-Nov-21 09:39:40 - INFO - Node for Small boats in Havana Harbour (record 01) created at http://localhost/node/33.
07-Nov-21 09:39:41 - INFO - Node for Manhatten Island (record 02) created at http://localhost/node/34.
07-Nov-21 09:39:42 - WARNING - Workbench exiting after receiving "ctrl c." Consult the documentation to learn how to resume your batch.
```

## Resuming a cancelled job/batch

The "documentation" you are referred to in the log snippet above is this section!

!!! note
    The best way to resume a cancelled job is to use Workbench's [recovery mode](/islandora_workbench_docs/recovery_mode/). But, if you can't use that (for example, your CSV ID to node ID map is not available), you can manually recover by following the instructions below.

The log snippet above shows that the last row in the CSV that resulted in a node being created is the row for record 02; after that row was processed, the user issued a `ctrl-c` to stop Workbench. To process the remaining CSV records ("resume your batch"), you need to remove from the input CSV the rows that were processed (according to the example log above, the rows for record 01 and record 02), and run Workbench on the resulting unprocessed records:

```text
file,id,title
IMG_2940.JPG,03,Looking across Burrard Inlet
IMG_2958.JPG,04,Amsterdam waterfront
IMG_5083.JPG,05,Alcatraz Island
```

Note that cancelling Workbench simply stops it from executing. It doesn't use a transaction to ensure that all child objects or media that are being processed are also processed:

* If it stops while creating a media associated with a node, it might stop before the media is created.
* If it stops while creating a compound object such as a book, it might stop before the children/pages are being processed.

If you cancel Workbench while it is running, you should always inspect the last object created and any of its media/children to ensure that they were all created. Use information in the log to see what was processed just prior to exiting.

!!! note
    You can also issue a `ctrl-c` while running a `--check`. If you do so, Workbench just logs the action and exits.
