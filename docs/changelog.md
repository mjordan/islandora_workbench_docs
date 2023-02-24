### main branch (no tag/release)

* Februray 24, 2023: Added `clean_csv_values_skip` config setting (commit e659616e, issue [567](https://github.com/mjordan/islandora_workbench/issues/567)).
* Februray 22, 2023: Resolved issue [563](https://github.com/mjordan/islandora_workbench/issues/563); Added `csv_value_templates` config setting (commit ae1fcd2b, issue [566](https://github.com/mjordan/islandora_workbench/issues/566)).
* Februray 20, 2023 (commit 96cc6ef): Resolved issue [554](https://github.com/mjordan/islandora_workbench/issues/554); (commit a143bab): resolved issue [556](https://github.com/mjordan/islandora_workbench/issues/556)).
* Februray 18, 2023 (commit ffa03de): Added `csv_headers` config option (issue [559](https://github.com/mjordan/islandora_workbench/issues/559)).
* Februray 16, 2023 (commit 9a8828b): Removed sample config files from workbench directory (issue [552](https://github.com/mjordan/islandora_workbench/issues/552)). Added new config option `log_term_creation` (commit 51348d0, issue [558](https://github.com/mjordan/islandora_workbench/issues/558)).
* Februray 15, 2023 (commit 309c311): Added `temp_dir` config option (issue [551](https://github.com/mjordan/islandora_workbench/issues/551)).
* Februray 14, 2023 (commit d200db6): Resolved issue (issue [553](https://github.com/mjordan/islandora_workbench/issues/553)).
* Febiruray 11, 2023 (commit 869bd5b): Resolved issue (issue [547](https://github.com/mjordan/islandora_workbench/issues/547)). Added `rollback_dir` config option (commit 1abad16, pull request [550](https://github.com/mjordan/islandora_workbench/pull/550)). Updated PR template (commit a32e88f).
* Februray 5, 2023 (commit 65db118): Resolved issue (issue [538](https://github.com/mjordan/islandora_workbench/issues/538)).
* January 31, 2023 (commit b452450): Resolved issue (issue [536](https://github.com/mjordan/islandora_workbench/issues/536)).
* January 29, 2023 (commit cff6008): Added ability to generate a contact sheet (issue [515](https://github.com/mjordan/islandora_workbench/issues/515)).
* January 26, 2023 (commit 6b0c16b): Added validation in `--check` of parent/child position in CSV file (issue [529](https://github.com/mjordan/islandora_workbench/issues/529)); resolved [issue 531](https://github.com/mjordan/islandora_workbench/issues/531) (commit 3150b4b).
* January 19, 2023 (commit b97b563): Fixed bug [522](https://github.com/mjordan/islandora_workbench/issues/522) and (commit 76d8c44) bug [523](https://github.com/mjordan/islandora_workbench/issues/523); changed log level from `ERROR` to `WARNING` when there are missing files and the `allow_missing_files` config option is set to true.
* January 18, 2023 (commit 727145f): Added `validate_parent_node_exists` config option (issue [521](https://github.com/mjordan/islandora_workbench/issues/521)).
* January 17, 2023 (commit a4a5008): Added better trimming of trailing slash in the `host` config option (issue [519](https://github.com/mjordan/islandora_workbench/issues/519)); (commit 1763fe6) fixed bug when "field_member_of" contained multiple values [520](https://github.com/mjordan/islandora_workbench/issues/520).
* January 15, 2023 (commit ba149d6d): Added validation of extensions for files named in the CSV `file` column (issue [126](https://github.com/mjordan/islandora_workbench/issues/126)); (commit 82dd02c) added validation of CSV values for "List (text)" type fields [509](https://github.com/mjordan/islandora_workbench/issues/509).
* January 9, 2023 (commit a3931df): Added ability to create media track files (issue [373](https://github.com/mjordan/islandora_workbench/issues/373)); fixed some integration tests.
* January 6, 2023 (commit f4e4c8d): Fixed issue [502](https://github.com/mjordan/islandora_workbench/issues/502).
* December 31, 2022: Better cleanup when using remote files - @ajstanley's fix for issue [497](https://github.com/mjordan/islandora_workbench/issues/497) (commit a0412af), resolved issue [499](https://github.com/mjordan/islandora_workbench/issues/497) (commit b8f74c8).
* December 28, 2022 (commit e4e6e49): Fixed bug where running Workbench using a Google Sheet or Excel file as input without first running `--check` caused a "file not found" error (issue [496](https://github.com/mjordan/islandora_workbench/issues/496)). Thanks to @ruebot for discovering this bug.
* December 11, 2022 (commit 24b70fd): Added ability to export files along with CSV data (issue [492](https://github.com/mjordan/islandora_workbench/issues/492)).
* December 5, 2022 (commit 0dbd459): Fixed bug in file closing when running `--check` during "get_data_from_view" tasks on Windows (issue [490](https://github.com/mjordan/islandora_workbench/issues/490)).
* November 28, 2022 (commit 46cfc34): Added quick delete option for nodes and media (issue [488](https://github.com/mjordan/islandora_workbench/issues/488)).
* November 24, 2022 (commit 3fe5c28): Extracted text media now have their "field_edited_text" field automatically populated with the contents of the specified text file (issue [407](https://github.com/mjordan/islandora_workbench/issues/407)).
* November 22, 2022 (commit 74a83cf): Added more detailed logging on node, media, and file creation (issue [480](https://github.com/mjordan/islandora_workbench/issues/480)).
* November 22, 2022 (commit f2a8a65): Added @DonRichards Dockerfile (PR [233](https://github.com/mjordan/islandora_workbench/pull/233)).
* November 16, 2022 (commit 07a74b2): Added new config options `path_to_python` and `path_to_workbench_script` (issue [483](https://github.com/mjordan/islandora_workbench/issues/483)).
* November 9, 2022 (commit 7c3e072): Fixed misspelling of "preprocessed" in code and temporary filenames (issue [482](https://github.com/mjordan/islandora_workbench/issues/482)).
* November 1, 2022 (commit 7c3e072): Workbench now exits when run withouth `--check` and there are no records in the input CSV (issue [481](https://github.com/mjordan/islandora_workbench/issues/481)).
* September 19, 2022 (commit 51c0f79): Replaced `exit_on_first_missing_file_during_check` configuration option with `strict_check` (issue [470](https://github.com/mjordan/islandora_workbench/issues/470)). `exit_on_first_missing_file_during_check` will be available until Nov. 1, 2022, at which time `strict_check` will be the only option allowed.
* September 18, 2022 (commit 00f50d6): Added ability to tell Workench to only process a subset of CSV records (issue [468](https://github.com/mjordan/islandora_workbench/issues/468)).
* September 1, 2022 (commit 6aad517): All hook scripts now log their exit codes (issue [464](https://github.com/mjordan/islandora_workbench/issues/464)).
* August 16, 2022 (commit 4270d13): Fixed bug that would not delete media with no files (issue [460](https://github.com/mjordan/islandora_workbench/issues/460)).
* August 13, 2022 (commit 1b7b801): Added ability to run shutdown scripts (issue [459](https://github.com/mjordan/islandora_workbench/issues/459)).
* August 12, 2022 (commit b821533): Provided configuration option `standalone_media_url: true` for sites who have Drupal's "Standalone media URL" option enabled (issue [466](https://github.com/mjordan/islandora_workbench/issues/466)).
* August 11, 2022 (commit df0a609): Fixed bug where items in secondary task CSV were created even if they didn't have a parent in the primary CSV, or if their parent was not created (issue [458](https://github.com/mjordan/islandora_workbench/issues/458)). They are now skipped.
* July 28, 2022 (commit 3d1753a): Added option to prompt user for password (issue [449](https://github.com/mjordan/islandora_workbench/issues/449); fixed 'version' in setup.py).
* July 27, 2022 (commit 029cb6d): Shifted to using Drupal's default media URIs (issue [446](https://github.com/mjordan/islandora_workbench/issues/446)).
* July 26, 2022 (commit 8dcf85a): Fixed setup.py on macOS/Homebrew (isue [448](https://github.com/mjordan/islandora_workbench/pull/448)).
* July 26, 2022 (commit 09e9f53): Changed license in setup.py to "MIT".

### Documentation

* February 24, 2023: Added "[How Workbench cleans your input data](/islandora_workbench_docs/preparing_data/#how-workbench-cleans-your-input-data)".
* February 22, 2023: Updated "[Preparing your data](/islandora_workbench_docs/preparing_data/)"; added "[CSV value templates](/islandora_workbench_docs/csv_value_templates/)".
* February 20, 2023: Updated "[Rolling back nodes and media](/islandora_workbench_docs/rolling_back/)".
* February 18, 2023: Updated "[Field data (Drupal and CSV)](/islandora_workbench_docs/fields/#using-field-labels-as-csv-column-headers)" to include new `csv_headers` setting.
* February 16, 2023: Updated "[Configuration](/islandora_workbench_docs/configuration/#logging-settings)" to include new `log_term_creation` setting.
* February 15, 2023: Updated "[Configuration](/islandora_workbench_docs/configuration/#miscellaneous-settings)" to include new `temp_dir` setting.
* February 11, 2023: Updated "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)" and "[Rolling back nodes and media](/islandora_workbench_docs/rolling_back/)."
* February 5, 2023: Updated the "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)" method of creating compound/paged content to explain using the new `page_title_template` config option.
* January 31, 2023: Updated "[Generating a contact sheet](/islandora_workbench_docs/contact_sheet)"; updated "[Configuring media types](/islandora_workbench_docs/media_types/)".
* January 30, 2023: Edits to the "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)" method of creating compound/paged content to clarify the absence of the "file" CSV column.
* January 29, 2023: Added "[Generating a contact sheet](/islandora_workbench_docs/contact_sheet)".
* January 23, 2023: Added example CSVs for primary and secondary tasks in the "[Case study](/islandora_workbench_docs/workflows/#case-study)" section of the Workflows documentation.
* January 22, 2023: Several clarifications and corrections, including @rosiel's correction of how to use `allow_missing_files` and `additional_files` together; added some [examples of planning large compound/paged content ingests](/islandora_workbench_docs/paged_and_compound/#with-pagechild-level-metadata).
* January 16, 2023: Updated "[Exporting Islandora 7 content](/islandora_workbench_docs/exporting_islandora_7_content/)."
* January 15, 2023: Updated "[Checking configuration and input data](/islandora_workbench_docs/check/)."
* January 9, 2023: Added docs for creating "[Media track files](/islandora_workbench_docs/media_track_files/)."
* January 8, 2023: Updated "[Known limitations](/islandora_workbench_docs/limitations/)" with a work around for unsupported "Filter by an entity reference View" fields; added examples of valid Windows paths to "[Values in the 'file' column](/islandora_workbench_docs/fields/#values-in-the-file-column)."
* December 29, 2022: Minor corrections to "[Known limitations](/islandora_workbench_docs/limitations/)", "[Workflows](/islandora_workbench_docs/workflows/)", "[Creating paged, compound, and collection content](/islandora_workbench_docs/paged_and_compound/)," and "[Preparing your data](/islandora_workbench_docs/preparing_data/)."
* December 28, 2022: Added cross reference between "[CSV field templates](/islandora_workbench_docs/field_templates/)" and "[Ignoring CSV rows and columns](/islandora_workbench_docs/ignoring_csv_rows_and_columns/#ignoring-csv-columns)".
* December 17, 2022: Corrected URI for `http://pcdm.org/use#OriginalFile` on "[Generating CSV files](/islandora_workbench_docs/generating_csv_files/)" and "[Configuration](/islandora_workbench_docs/configuration/)."
* December 11, 2022: Updated "[Generating CSV files](/islandora_workbench_docs/generating_csv_files/)" and "[Output CSV settings](/islandora_workbench_docs/configuration/#output-csv-settings)" Configuration docs to include new ability to export files along with CSV data. Note: the `data_from_view_file_path` setting in "get_data_from_view" tasks has been replaced with `export_csv_file_path`.
* November 28, 2022: Added "[Quick delete](/islandora_workbench_docs/quick_delete/)" docs; added clarification to "[Configuring Drupal's media URLs](/islandora_workbench_docs/installation/#configuring-drupals-media-urls)" that `standalone_media_url: true` must be in all config files for tasks that interact with media; added note to "[Adding media to nodes](/islandora_workbench_docs/adding_media/)" and "[Values in the 'file' column](/islandora_workbench_docs/fields/#values-in-the-file-column)" clarifying that it is not possible to override the filesystem a media's file field is configured to use.
* November 26, 2022: Changed documentation theme from readthedocs to material; some edits for clarity to the docs for ["file" field values](/islandora_workbench_docs/fields/#values-in-the-file-column); some edits for clarity to the docs for "[adaptive pause](/islandora_workbench_docs/reducing_load/#adaptive-pause)."
* November 24, 2022: Added note to "[Adding media to nodes](/islandora_workbench_docs/adding_media/)" and "[Adding multiple media](/islandora_workbench_docs/adding_multiple_media/)" about extracted text media; added a note about using absolute file paths in scheduled jobs to the "[Workflows](/islandora_workbench_docs/workflows/)" and "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)"; removed the "required" ✔️ from the `password` configuration setting entry in the table in "[Configuration](/islandora_workbench_docs/configuration/)".
* November 17, 2022: Added new config options `path_to_python` and `path_to_workbench_script` to "[Congfiguration](/islandora_workbench_docs/configuration/)" docs.
* October 28, 2022: Updated "[Congfiguration](/islandora_workbench_docs/configuration/)" docs to provide details on YAML (configuration file) syntax.
* September 19, 2022: Updated references to `exit_on_first_missing_file_during_check` to use `strict_check`. Configuration settings entry advises `exit_on_first_missing_file_during_check` will be removed Nov. 1, 2022.
* September 18, 2022: Added entry "[Ignoring CSV rows and columns](/islandora_workbench_docs/ignoring_csv_rows_and_columns)."
* September 15, 2022: Added entry to "[Limitations](/islandora_workbench_docs/limitations/)" page about lack of support for HTML markup. Also added a section on "Password management" to "[Requirements and installation](/islandora_workbench_docs/installation/)".
* September 8, 2022: Added documentation on "[Reducing Workbench's impact on Drupal](/islandora_workbench_docs/reducing_load/)."
* August 30, 2022: Updated "[Hooks](/islandora_workbench_docs/hooks/)" docs to clarify that the HTTP response code passed to post-entity-create scripts is a string, not an integer.
* August 18, 2022: Updated `standalone_media_url` entry in the "[Congfiguration](/islandora_workbench_docs/configuration/)" docs, and added brief entry to the "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)" page about clearning Drupal's cache.
* August 13, 2022: Updated "[Congfiguration](/islandora_workbench_docs/configuration/)" and "[Hooks](/islandora_workbench_docs/hooks/)" page to describe shutdown scripts.
* August 11, 2022: Added text to "[Creating paged, compound, and collection content](/islandora_workbench_docs/paged_and_compound/#using-a-secondary-task)" page to clarify what happens when a row in the secondary CSV does not have a matching row in the primary CSV.
* August 8, 2022: Added entry to "[Limitations](/islandora_workbench_docs/limitations/)" page about support for "Filter by an entity reference View" fields.
* August 3, 2022: Added entry to "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)" page about missing Microsoft Visual C++ error when installing Workbench on Windows.
* August 3, 2022: Updated the "[Limitations](/islandora_workbench_docs/limitations/)" page with entry about Paragraphs.
* August 2, 2022: Added note about ownership requirements on files to "[Deleting nodes](/islandora_workbench_docs/deleting_nodes/)"; was previously only on "Deleting media".
* July 28, 2022: Updated `password` entry in the "[Congfiguration](/islandora_workbench_docs/configuration/)" docs to mention the new password prompt feature.
