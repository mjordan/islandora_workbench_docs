You may also be interested in change log items [prior to 2025](/islandora_workbench_docs/changelog_up_to_2024/).

### main branch (no tag/release)

* September 16, 2025: Resolved (commit c1ae668) [issue 777](https://github.com/mjordan/islandora_workbench/issues/777) and (commit 4228c9b) [issue 1015](https://github.com/mjordan/islandora_workbench/issues/1015).
* August 5, 2025: Merged [PR 1003](https://github.com/mjordan/islandora_workbench/pull/1003) (related to [issue 963](https://github.com/mjordan/islandora_workbench/issues/963)).
* July 22, 2025: Resolved (commit 9c2552d) [issue 998](https://github.com/mjordan/islandora_workbench/issues/998).
* July 15, 2025: Resolved (commit afadce5) [issue 995](https://github.com/mjordan/islandora_workbench/issues/995).
* July 10, 2025: Added `scripts/tile_warmer.py`; resolved (commit 43d675) [issue 982](https://github.com/mjordan/islandora_workbench/issues/982) and (commit 4f02c28) [issue 973](https://github.com/mjordan/islandora_workbench/issues/973).
* July 9, 2025: Resolved (commit adcea1d) [issue 990](https://github.com/mjordan/islandora_workbench/issues/990) and (commit e2ababe) [issue 991](https://github.com/mjordan/islandora_workbench/issues/991).
* July 7, 2025: Resolved (commit 551798f) [issue 986](https://github.com/mjordan/islandora_workbench/issues/986).
* July 4, 2025: Resolved (commit ad0f22d) [issue 956](https://github.com/mjordan/islandora_workbench/issues/956).
* July 2, 2025: Resolved (commit 890f75e) [issue 981](https://github.com/mjordan/islandora_workbench/issues/981) and (committ eb2e4c4) [issue 979](https://github.com/mjordan/islandora_workbench/issues/979).
* June 25, 2025: Resolved (commit 17e5844) [issue 965](https://github.com/mjordan/islandora_workbench/issues/965).
* June 25, 2025: Resolved (commit df52714) [issue 976](https://github.com/mjordan/islandora_workbench/issues/976).
* June 23, 2025: Resolved (commit bd60ae1) [issue 969](https://github.com/mjordan/islandora_workbench/issues/969). Thank you @ysuarez!
* June 19, 2025: Resolved (commit 42e6fcd) [issue 949](https://github.com/mjordan/islandora_workbench/issues/949) and [issue 971](https://github.com/mjordan/islandora_workbench/issues/971).
* June 18, 2025: Resolved (commit 97fc6e65) [issue 338](https://github.com/mjordan/islandora_workbench/issues/338) and (commit 97fc6e6) [issue 958](https://github.com/mjordan/islandora_workbench/issues/958).
* June 17, 2025: Resolved (commit 087c707) [issue 962](https://github.com/mjordan/islandora_workbench/issues/962), (commit e506f92) [issue 957](https://github.com/mjordan/islandora_workbench/issues/957), and (commit a070a96) [issue 968](https://github.com/mjordan/islandora_workbench/issues/968); fixed some broken tests.
* June 16: 2025: Commented out logic in workbench to enable check for updates introduced in commit fc74be4be (needs more testing). Fixed (commit 25aea6f) behavior of `delete_zip_archive_after_extraction` config setting so it doesn't delete local .zip archives on `--check`.
* June 11, 2025: Resolved (commit b852280) [issue 893](https://github.com/mjordan/islandora_workbench/issues/893) and (commit fc74be4) [issue 338](https://github.com/mjordan/islandora_workbench/issues/338).
* June 3, 2025: Resolved (commit 2595490) [issue 953](https://github.com/mjordan/islandora_workbench/issues/953).
* May 31, 2025: Resolved (commit 8b59144) [issue 952](https://github.com/mjordan/islandora_workbench/issues/952).
* May 29, 2025: Resolved (commit e89ebe6) [issue 951](https://github.com/mjordan/islandora_workbench/issues/951).
* May 19, 2025: Resolved (commit f44916c) [issue 948](https://github.com/mjordan/islandora_workbench/issues/948).
* May 9, 2025: Merged [PR 922](https://github.com/mjordan/islandora_workbench/pull/922) (refactor field handler classes) and [PR 930](https://github.com/mjordan/islandora_workbench/pull/930) (adds a linked data field type). Thanks very much @whikloj!
* May 8, 2025: Additional work on [issue 895](https://github.com/mjordan/islandora_workbench/issues/714) (commit 5977be0): added ability to configure a subset of input CSV for `add_media` tasks.
* May 5, 2025: Resolved (commit b09b116) [issue 946](https://github.com/mjordan/islandora_workbench/issues/946) (Thanks @alxp!); merged (commit e02528d) [PR 940](https://github.com/mjordan/islandora_workbench/pull/940). Thanks @noahwsmith!
* May 1, 2025: Resolved (commit 4c44e30) [issue 714](https://github.com/mjordan/islandora_workbench/issues/714).
* April 30, 2025: Resolved (commit 89698d0) [issue 945](https://github.com/mjordan/islandora_workbench/issues/945).
* April 27, 2025: Merged (commit df1f263) in changes to introduce `add_alt_text` and `update_alt_text` tasks.
* April 24, 2025: (Commit 71ba537) added `show_percentage_of_csv_input_processed` setting to `add_media` tasks.
* April 23, 2025: Merged (commit b7b009a) [PR 942](https://github.com/mjordan/islandora_workbench/pull/942) that adds the ability to export multiple media files in `export_csv` and `get_data_from_view` tasks. Thanks @alxp!
* April 8, 2025: Merged (commit 7d8be02) fix for [issue 937](https://github.com/mjordan/islandora_workbench/issues/937) (thanks @alxp!).
* April 3, 2025: Merged (commit e65062c) fix for [issue 928](https://github.com/mjordan/islandora_workbench/issues/932) (thanks @mitchmac and @dara2!); resolved (commit e65062c) [issue 935](https://github.com/mjordan/islandora_workbench/issues/935).
* March 28, 2025: Resolved (commit 189b025) [issue 932](https://github.com/mjordan/islandora_workbench/issues/932).
* March 25, 2025: Resolved (commit e5b5576) [issue 918](https://github.com/mjordan/islandora_workbench/issues/918) and (commit 561dfc3) [issue 919](https://github.com/mjordan/islandora_workbench/issues/919). Thanks @whikloj!
* March 4, 2025: Resolved (commit 11ab1e) [issue 915](https://github.com/mjordan/islandora_workbench/issues/915).
* February 28, 2025: Resolved (commit 07b54b3) [issue 909](https://github.com/mjordan/islandora_workbench/issues/909).
* February 24, 2025: Resolved (commit aecb50e) [issue 894](https://github.com/mjordan/islandora_workbench/issues/894) and (comit 73bd6ba) [issue 902](https://github.com/mjordan/islandora_workbench/issues/902).
* February 23, 2025: Resolved (commit 8f7f03d) [issue 899](https://github.com/mjordan/islandora_workbench/issues/899).
* February 18, 2025: Resolved (commit 859fc16) [issue 896](https://github.com/mjordan/islandora_workbench/issues/896), (commit 0eb524c) [issue 900](https://github.com/mjordan/islandora_workbench/issues/900), and (commit 850f7fd) [issue 901](https://github.com/mjordan/islandora_workbench/issues/901).
* February 13, 2025: Resolved (commit 8de8951) [issue 886](https://github.com/mjordan/islandora_workbench/issues/886) and (commit 9da97fb) [issue 877](https://github.com/mjordan/islandora_workbench/issues/877).
* February 7, 2025: Resolved (commit cb25ee9) [issue 872](https://github.com/mjordan/islandora_workbench/issues/872) and [issue 878](https://github.com/mjordan/islandora_workbench/issues/878).
* February 3, 2025: Resolved (commit 57d9de1) [issue 883](https://github.com/mjordan/islandora_workbench/issues/883) and (commit 95eaef3) [issue 884](https://github.com/mjordan/islandora_workbench/issues/884).
* January 30, 2025: Merged in fix for [issue 882](https://github.com/mjordan/islandora_workbench/issues/882). Thanks Born-Digital!
* January 16, 2025: Resolved (commit 3c58532) [issue 747](https://github.com/mjordan/islandora_workbench/issues/747). Thank you @joecorall!
* January 12, 2025: Resolved (commit 49853cb) [issue 875](https://github.com/mjordan/islandora_workbench/issues/875).
* January 9, 2025: Resolved (commit 8fb1290) [issue 873](https://github.com/mjordan/islandora_workbench/issues/873).
* January 1, 2025: Resolved (commit f8dce4c) [issue 799](https://github.com/mjordan/islandora_workbench/issues/799).


### Documentation

* September 18, 2025: Added "[Session cookies for remote files](/islandora_workbench_docs/session_cookies_for_remote_files/)" and updated the configuration settings page with the new `remote_file_cookie_name` and `remote_file_cookie_value` settings.
* September 16, 2025: Updated the "[Configuration](/islandora_workbench_docs/configuration/#media-settings)" page to add the new `keep_filename_parent_directory` config setting.
* July 31, 2025: Updated the "[The CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/#host-values-in-the-map)" page to clarify when you should use the  `csv_id_to_node_id_map_allowed_hosts` config setting.
* July 22, 2025: Updated the "[Development guide](/islandora_workbench_docs/development_guide/#islandora-workbench-integration-drupal-module)" to describe when to modify `check_integration_module_version()`.
* July 16, 2025: Broke out the "Password management" section of "[Requirements and installation](/islandora_workbench_docs/installation/)" out into a separate page, "[User management](/islandora_workbench_docs/user_management/)".
* July 15, 2025: Updated "[Password management](/islandora_workbench_docs/installation/#password-management)" docs to include the new `credentials_file_path` setting; updated "[Generating CSV files and exporting Islandora content](/islandora_workbench_docs/generating_csv_files/)" to reflect changes in [issue 973](https://github.com/mjordan/islandora_workbench/issues/973).
* July 10, 2025: Updated "[Running scripts](/islandora_workbench_docs/running_scripts/)" to include example of `tile_warmer.py`.
* July 9, 2025: Updated "[Ignoring CSV rows and columns](/islandora_workbench_docs/ignoring_csv_rows_and_columns/)" and "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)" to note the new warning about conflicting input-CSV-filtering config settings.
* July 4, 2025: Added docs on "[Updating media using node IDs](/islandora_workbench_docs/updating_media/#updating-media-using-node-ids)".
* July 3, 2025, Updated several pages to include reference to the new `run_scripts` task.
* July 2, 2025: Added docs on the new `run_scripts` task at "[Running scripts](/islandora_workbench_docs/running_scripts/)". Also updated docs to use single quotes around Windows file paths, and added a new section to the [Troubleshooting page](/islandora_workbench_docs/troubleshooting/#im-running-workbench-on-windows-and-im-having-problems-getting-long-directory-paths-right-in-my-configuration-file) about this.
* June 26, 2025: Updated the "[Workflows](/islandora_workbench_docs/workflows/#using-hooks)" page to describe populating a persisent queue ([issue 976](https://github.com/mjordan/islandora_workbench/issues/976)).
* June 20, 2025: Added ""[Deferring Solr indexing](/islandora_workbench_docs/reducing_load/#deferring-solr-indexing)" to the "Reducing Workbench's impact on Drupal" page; added mention of the new check for whether Workbench is up to date to the "[Optimizing your use of Workbench](/islandora_workbench_docs/optimizing_your_use_of_workbench)" page.
* June 19, 2025: Updated the "[Creating paged, compound, and collection content](/islandora_workbench_docs/paged_and_compound/#ignoring-files-in-page-directories)" page to update socs on using the `paged_content_ignore_files` config setting.
* June 18, 2025: Updated the "[Configuration](/islandora_workbench_docs/configuration/#miscellaneous-settings)" page to add the `check_for_workbench_updates` config setting.
* June 16, 2025: Updated docs on the `delete_zip_archive_after_extraction` config setting to clarify that it applies only to remote .zip archive files, not local ones.
* June 12, 2025: Added "[Printing all configuration values](/islandora_workbench_docs/configuration/#printing-all-configuration-values)" section to the docs. Updated the "[Troubleshooting](/islandora_workbench_docs/troubleshooting/#workbench-is-slow)" page to suggest deferring indexing to speed up `create` and `update` tasks.
* June 11, 2025: Updated the "[The CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/#host-values-in-the-map)" page to add a section on using the new `csv_id_to_node_id_map_allowed_hosts` config setting.
* June 9, 2025: Updated the "[Rolling back nodes and media](/islandora_workbench_docs/rolling_back/)" page to add a section on rolling back a subset of the nodes identified in the rollback.csv file.
* June 4, 2025: Updated the "[Configuration](/islandora_workbench_docs/configuration/#miscellaneous-settings)" page to add the `recovery_mode_starting_from_node_id` config setting.
* June 3, 2025: Updated "[Generating CSV files](/islandora_workbench_docs/generating_csv_files/)" to include changes made in resolving [issue 953](https://github.com/mjordan/islandora_workbench/issues/953).
* May 6, 2025: Added new "[Using Workbench within a Docker container](/islandora_workbench_docs/installation/#using-workbench-within-a-docker-container)" docs.
* May 1, 2025: Added new "[Automatically populating the "Viewer override" field](/islandora_workbench_docs/viewer_override/)" page; broke some config settings into a new "[Input CSV content templating settings](/islandora_workbench_docs/configuration/#input-csv-content-templating-settings)" section.
* April 27, 2025: Updated "[Adding alt text to images](/islandora_workbench_docs/alt_text/)" and associated pages to document the new `add_alt_text` and `update_alt_text` tasks.
* April 24, 2025: Updated "[Configuration](/islandora_workbench_docs/configuration/)" entry for the `show_percentage_of_csv_input_processed` config setting.
* April 23, 2025: Updated [Generating CSV Files](/islandora_workbench_docs/generating_csv_files/) to reflect new functionality around exporting media files.
* April 8, 2025: Merged section into "[Installation](/islandora_workbench_docs/installation/)" on preferred way to install Islandora Workbench on macOS. Thanks @alxp! Also clarified use of the `media_types_override` and `media_type_by_media_use` config settings in the "[Configuring media types](/islandora_workbench_docs/media_types/)" and "[Adding multiple media](/islandora_workbench_docs/adding_multiple_media/)" pages.
* April 3, 2025: Added entry to the "[Configuration](/islandora_workbench_docs/configuration/#paged-and-compound-content-settings)" page for the new `viewer_override_fieldname` config setting.
* April 1, 2025: Replaced outdated "field_display_hints" with "field_viewer_override".
* March 4, 2025: Clarified where the `path_to_python` and `path_to_workbench_script` config settings go - the config file for the primary task. Thanks for the suggestion @edlington!
* March 3, 2025: Fixed a typo on the "[Configuration](/islandora_workbench_docs/configuration/)" page.
* March 1, 2025: Merged in fixes to docs for [get_islandora_7_content.py](/islandora_workbench_docs/exporting_islandora_7_content/) by @whikloj. Thanks!
* February 28, 2025: Added "[The remove_password_from_config_file configuration setting](/islandora_workbench_docs/installation/#the-remove_password_from_config_file-configuration-setting)".
* February 27, 2025: Added "[Optimizing your use of Workbench](/islandora_workbench_docs/optimizing_your_use_of_workbench/)".
* February 24, 2025: Updated docs on "[Deleting nodes](/islandora_workbench_docs/deleting_nodes/)".
* February 23, 2025: Updated docs on "[Configuration](/islandora_workbench_docs/configuration/)" to include new `show_percentage_of_csv_input_processed` setting.
* February 18, 2025: Updated docs on "[Processing specific CSV rows](/islandora_workbench_docs/ignoring_csv_rows_and_columns/#processing-specific-csv-rows)"; updated "[Reducing Workbench's impact on Drupal](/islandora_workbench_docs/reducing_load/)" and the [HTTP configuration settings](/islandora_workbench_docs/configuration/#http-settings).
* February 16, 2025: Updated docs on [using Paragraph fields](/islandora_workbench_docs/fields/#using-paragraph-fields-in-create_terms-and-update_terms-tasks) in `create_terms` and `update_terms` tasks. Thanks @dara2!
* February 15, 2025: Added documentation on ingesting large files, mainly pointing to [Islandora's documentation](https://islandora.github.io/documentation/user-documentation/uploading-large-files/) on this topic; updated "[Using CSV row ranges](/islandora_workbench_docs/ignoring_csv_rows_and_columns/#using-csv-row-ranges)" to clarify that `csv_start_row` and `csv_stop_row` only apply to `create` and `update` tasks.
* February 14, 2025: Moved change log entries up to the end of 2024 into their [own page](/islandora_workbench_docs/changelog_up_to_2024/).
* February 10, 2025: Added "[Recovering from interrupted create tasks](/islandora_workbench_docs/recovery_mode/)" and updated associated other pages; moved "[The CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/)" from "[Generating CSV Files](/islandora_workbench_docs/generating_csv_files/)" into its own page.
* February 7, 2025: Fixed the broken "Note" on the section on using [Paragraph fields](/islandora_workbench_docs/fields/#paragraphs-entity-reference-revisions-fields).
* February 3, 2025: Added `completion_message` to the "Miscellaneous" section of the "[Configuration settings](/islandora_workbench_docs/configuration/#miscellaneous-settings)" list.
* January 22, 2025: Updated "[Requirements and installation](/islandora_workbench_docs/installation/)" to document preferred way of installing libraries used by Workbench (issue [879](https://github.com/mjordan/islandora_workbench/issues/879)).
* January 16, 2025: Updated "[Requirements and installation](/islandora_workbench_docs/installation/)" and "[Development guide](/islandora_workbench_docs/development_guide/)" to reflect changes introduced in [merge request 849](https://github.com/mjordan/islandora_workbench/pull/849) (adding CI integration tests). Removed the Roadmap since it was out of date.
* January 6, 2025: Updated docs on "[Using the CSV ID to node ID map](islandora_workbench_docs/generating_csv_files/#using-the-csv-id-to-node-id-map)".
* January 6, 2025: Updated docs on "[Checking if nodes already exist](/islandora_workbench_docs/checking_if_nodes_exist/)".
* January 1, 2025: Updated docs on "[Rolling back nodes and media](/islandora_workbench_docs/rolling_back/)" to include new settings added in [issue 868](https://github.com/mjordan/islandora_workbench/issues/868).
