You may also be interested in change log items [prior to 2025](/islandora_workbench_docs/changelog_up_to_2024/).

### main branch (no tag/release)

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

* April 8, 2025: Merged section into "[Installation](/islandora_workbench_docs/installation/)" on preferred way to install on macOS. Thanks @alxp! Also clarified use of the `media_types_override` config setting in the "[Configuring media types](/islandora_workbench_docs/media_types/)" and "[Adding multiple media](/islandora_workbench_docs/adding_multiple_media/)" pages.
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
