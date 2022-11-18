### main branch (no tag/release)

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
