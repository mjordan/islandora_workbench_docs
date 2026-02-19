If you encounter a problem, take a look at the "things that might sound familiar" section below. But, if the problem you're encountering isn't described below, you can ask for help.

## Ask for help

The `#islandoraworkbench` Slack channel is a good place to ask a question if Workbench isn't working the way you expect or if it crashes. You can also [open a Github issue](https://github.com/mjordan/islandora_workbench/issues).

If Workbench "isn't working the way you expect", the documentation is likely unclear. Crashes are usually caused by sloppy Python coding. Reporting either is a great way to contribute to Islandora Workbench.

## But before you ask...

The first step you should take while troubleshooting a Workbench failure is to use Islandora's graphical user interface to create/edit/delete a node/media/taxonomy term (or whatever it is you're trying to do with Workbench). If Islandora works without error, you have confirmed that the problem you are experiencing is likely isolated to Workbench and is not being caused by an underlying problem with Islandora.

Next, if you have eliminated Islandora as the cause of the Workbench problem you are experiencing, you might be able to fix the problem by pulling in the most recent Workbench code. The best way to keep it up to date is to pull in the latest commits from the Github repository periodically, but if you haven't done that in a while, within the "islandora_workbench" directory, run the following git commands:

1. `git branch`, which should tell whether you're currently in the "main" branch. If you are:
1. `git pull`, which will fetch the most recent code and and merge it into the code you are running.

If git tells you it has pulled in any changes to Workbench, you will be running the latest code. If you get an error while running git, ask for help.

Also, you might be asked to provide one or more of the following:

* your configuration file (without username and password!). You can also print your configuration to your terminal by includeing the `--print_config` argument to workbench, e.g. `python workbench --config test.yml --check --print_config`.
* some sample input CSV
* your Workbench log file
* details from your Drupal log, available at Admin > Reports > Recent log messages
* whether you have certain contrib modules installed, or other questions about how your Drupal is configured.


## Some things that might sound familiar

### Running Workbench results in lots of messages like `InsecureRequestWarning: Unverified HTTPS request is being made to host 'islandora.dev'`.

If you see this, and you are using an ISLE istallation whose Drupal hostname uses the `traefik.me` domain (for example, https://islandora.traefik.me), the HTTPS certificate for the domain has expired. This problem will be widespread so please check the Islandora Slack for any current discussion about it. If you don't get any help in Slack, try redownloading the certificates following the instructions in the [Islandora documentation](https://islandora.github.io/documentation/installation/docker-custom/#traefikme-ssl-certificate-expired-or-revoked). If that doesn't work, you can temporarily avoid the warning messages by

1. adding `secure_ssl_only: false` to your Workbench config file and
2. updating an environment variable using the following command: `export PYTHONWARNINGS="ignore:Unverified HTTPS request"`


### Workbench is slow.

True, it can be slow. However, users have found that the following strategies increase Workbench's speed substantially:

* Running Workbench on the same server that Drupal is running on (e.g. using "localhost" as the value of `host` in your config file). While doing this negates Workbench's most important design principle - that it does not require access to the Drupal server's command line - during long-running jobs such as those that are part of migrations, this is the best way to speed up Workbench.
* Defer Solr indexing. Configuring Solr to not index newly added or updated items immediately can result in noticably faster `create` and `update` jobs. To do this, visit `/admin/config/search/search-api/index/default_solr_index/edit` and in the "Index options" fieldset, make sure "Index items immediately" is unchecked and save the form. Indexing will happen during cron runs.
* Use term IDs, and not terms names or URIs, in your input CSV. When creating and updating nodes and other entities, Drupal requires term IDs. If you use term names or term URIs in your input CSV, Workbench does a lookup against Drupal to get the corresponding term ID. If you use raw term IDs in your input CSV, you can avoid this lookup.
* Using local instead of remote files. If you populate your `file` or "additional files" fields with filenames that start with "http", Workbench downloads each of those files before ingesting them. Providing local copies of those files in advance of running Workbench will eliminate the time it takes Workbench to download them.
* Avoid confirming taxonomy terms' existence during `--check`. If you add `validate_terms_exist: false` to your configuration file, Workbench will not query Drupal for each taxonomy term during `--check`. This option is suitable if you know that the terms don't exist in the target Drupal. Note that this option only speeds up `--check`; it does not have any effect when creating new nodes.
* Generate FITS XML files offline and then add them as media like any other file. Doing this allows Islandora to not generate FITS data, which can slow down ingests substantially. If you do this, be sure to disable the "FITS derivatives" Context first.

### I've pulled in updates to Islandora Workbench from Github but when I run it, Python complains about not being able to find a library.

This won't happen very often, and the cause of this message will likely have been announced in the `#islandoraworkbench` Slack channel. This error is caused by the addition of a new Python library to Workbench. Running `setup.py` will install the missing library. Details are available in the "Updating Islandora Workbench" section of the [Requirements and Installation](https://mjordan.github.io/islandora_workbench_docs/installation/#updating-islandora-workbench) docs.

You *do not need* to run `setup.py` every time you update the Workbench code. Introducing a new library is not a common occurance.

### I'm running Workbench on Windows and I'm having problems getting long directory paths right in my configuration file.

Workbench, thanks to Python's excellent cross-operating system handling of directory paths, can understand Windows paths reliably as long as they are wrapped in *single* quotation marks. Double quotation marks tells Python to treat a `\`, which is common in Windows paths, as an escape character. If you wrap your Windows paths in single quotation marks, the `\` is not seen as an escape character.

Also, Workbench doesn't care if you use backslashes (`\`) or forward slashes (`/`) in the paths if you wrap paths in single quotation marks. For example, paths like `'d:\data/workarea\collection_23'` and `'\\some.windows.file.share.edu\share name\files\myfile.png'` will work fine.

### Workbench is failing to ingest some nodes and is leaving messages in the log mentioning HTTP response code 422.

This is probably caused by unexpected data in your CSV file that Workbench's `--check` validation is not finding. If you encounter these messages, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and share any relevant entries in your Workbench log and Drupal log (as an admin user, go to Admin > Reports > Recent log messages) so we can track down the problem.

One of the most common causes of this error is that one or more of the vocabularies being populated in your `create` task CSV contain required fields other than the default term name. It is possible to have Workbench create these fields, but you must do so as a separate `create_terms` task. See "[Creating taxonomy terms](/islandora_workbench_docs/creating_taxonomy_terms)" for more information.

### I can't upload large files

The maximum size of a file that can be ingested by Workbench is determined by configuration settings in PHP, Drupal and Fedora. The settings that need to be adjusted are documented in the [Islandora documentation](https://islandora.github.io/documentation/user-documentation/uploading-large-files/). That documentation drew on the experience of Workbench users as shared in [this Github issue](https://github.com/mjordan/islandora_workbench/issues/353). If you need additional assistance configuring your server to ingest large files, ask for help on the Islandora Slack.


### Workbench is crashing and telling me there are problems with SSL certificates.

To determine if this issue is specific to Workbench, from the same computer Workbench is running on, try hitting your Drupal server (or server your remote files are on) with `curl`. If `curl` also complains about SSL certificates, the problem lies in the SSL/HTTPS configuration on the server. An example `curl` command is `curl https://wwww.lib.sfu.ca`.

If `curl` doesn't complain, the problem is specific to Workbench.

### --check is telling me that one the rows in my CSV file has more columns than headers.

The most likely problem is that one of your CSV values contains a comma but is not wrapped in double quotes.

### My Drupal has the "Standalone media URL" option at `/admin/config/media/media-settings` checked, and I'm using Workbench's `standalone_media_url: true` option in my config, but I'm still getting lots of errors.

Be sure to clear Drupal's cache every time you change the "Standalone media URL" option. More information can be found [here](/islandora_workbench_docs/installation/#configuring-drupals-media-urls).

### Workbench crashes or slows down my Drupal server.

If Islandora Workbench is putting too much strain on your Drupal server, you should try enabling the `pause` configuration option. If that works, replace it with the `adaptive_pause` option and see if that also works. The former option pauses between all communication requests between Workbench and Drupal, while the latter pauses only if the server's response time for the last request is longer than the average of the last 20 requests.

Note that both of these settings will slow Workbench down, which is their purpose. However, `adaptive_pause` should have less impact on overall speed since it only pauses between requests if it detects the server is getting slower over time. If you use `adaptive_pause`, you can also tune the `adaptive_pause_threshold` option by incrementing the value by .5 intervals (for example, from the default of 2 to 2.5, then 3, etc.) to see if doing so reduces strain on your Drupal server while keeping overall speed acceptable. You can also lower the value of `adaptive_pause` incrementally to balance strain with overall speed.

### Workbench thinks that a remote file is an .html file when I know it's a video (or audio, or image, etc.) file.

Some web applications, including Drupal 7, return a human-readable HTML page instead of a expected HTTP response code when they encounter an error. If Workbench is complaining that a remote file in your `file` other file column in your input CSV has an extension of ".htm" or ".html" and you know that the file is not an HTML page, what Workbench is seeing is probably an error message. For example, Workbench might leave a message like this in your log: `Error: File "https://digital.lib.sfu.ca/islandora/object/edcartoons:158/datastream/OBJ/view" in CSV row "text:302175" has an extension (html) that is not allowed in the "field_media_file" field of the "file" media type.`

This error can be challenging to track down since the HTML error page might have been specific to the request that Workbench just made (e.g. a timeout or some other temporary server condition). One way of determining if the error is temporary (i.e. specific to the request) is to use `curl` to fetch the file (e.g., `curl -o test.tif https://digital.lib.sfu.ca/islandora/object/edcartoons:158/datastream/OBJ/view`). If the returned file (in this example, it will be named `test.tif`) is in fact HTML, the error is probably permanent or at least persistent; if the file is the one you expected to retrieve, the error was temporary and you can ignore it.

### The text in my CSV does not match how it looks when I view it in Drupal.

If a field is configured in Drupal to use [text filters](https://www.drupal.org/node/213156), the HTML that is displayed to the user may not be exactly the same as the content of the node add/edit form field. If you check the node add/edit form, the content of the field should match the content of the CSV field. If it does, it is likely that Drupal is apply a text filter. See [this issue](https://github.com/mjordan/islandora_workbench/issues/328) for more information.

### My Islandora uses a custom media type and I need to tell Workbench what file field to use.

If you need to create a media that is not one of the standard Islandora types (Image, File, Digital Document, Video, Audio, Extracted Text, or FITS Technical metadata), you will need to include the  `media_file_fields` setting in your config file, like this:


```yaml
media_file_fields:
 - mycustommedia_machine_name: field_custom_file
 - myothercustommedia_machine_name: field_other_custom_file
```

This configuration setting adds entries to the following default mapping of media types to file field names:

```yaml
'file': 'field_media_file',
'document': 'field_media_document',
'image': 'field_media_image',
'audio': 'field_media_audio_file',
'video': 'field_media_video_file',
'extracted_text': 'field_media_file',
'fits_technical_metadata': 'field_media_file'
```

### EDTF 'interval' values are not rendering in Islandora properly.

Islandora can display EDTF interval values (e.g., `2004-06/2006-08`, `193X/196X`) properly, but by default, the configuration that allows this is disabled (see [this issue](https://github.com/Islandora/documentation/issues/1889) for more information). To enable it, for each field in your Islandora content types that use EDTF fields, visit the "Manage form display" configuration for the content type, and for each field that uses the "Default EDTF widget", within the widget configuration (click on the gear), check the "Permit date intervals" option and click "Update":

![EDTF form widget configuration](images/edtf_form_widget_config.png)

### My CSV file has a `url_alias` column, but the aliases are not being created.

First thing to check is whether you are using the [Pathauto](https://www.drupal.org/project/pathauto) module. It also creates URL aliases, and since by default Drupal only allows one URL alias, in most cases, the aliases it creates will take precedence over aliases created by Workbench.

### Workbench is telling me my input CSV contains zero rows (or far fewer rows than it should)

This is likely caused by overlapping [input CSV filtering](/islandora_workbench_docs/ignoring_csv_rows_and_columns/) configuration settings, e.g. `csv_start_row`, `csv_stop_row`, `csv_rows_to_process`, or `csv_row_filters`, and/or the use of `#` to comment out rows. If your configuration file contains more than one of these settings, and you are using `#` to comment out rows, during `--check` Workbench will add a warning to your log file to check the preprocessed version of your CSV.

### I'm having trouble getting Workbench to work in a cronjob

The most common problem you will encounter when running Islandora Workbench in a cronjob is that Workbench can't find its configuration file, or input/output directories. The easiest way to avoid this is to use absolute file paths everywhere, including as the value of Workbench's `--config` parameter, in configuration files, and in `file` and `additional_files` columns in your input CSV files.

In some cases, particularly if you are using a secondary task to create pages or child items, you many need to use the `path_to_python` and `path_to_workbench_script` configuration settings.

### get_islandora_7_content.py crashes with the error "illegal request: Server returned status of 400. The default query may be too long for a url request."

Islandora 7's Solr contains a lot of redundant fields. You need to reduce the number of fields to export. See the "[Exporting Islandora 7 content](/islandora_workbench_docs/exporting_islandora_7_content/#configuring-which-solr-fields-to-include-in-the-csv)" documentation for ways to reduce the number of fields. Ask in the `#islandoraworkbench` Slack channel if you need help.
