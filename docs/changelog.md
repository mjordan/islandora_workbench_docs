You may also be interested in change log items recorded in [2025](/islandora_workbench_docs/changelog_2025/).

### main branch (no tag/release)

* January 26, 2026: Resolved (commit 12a0419) [issue 1055](https://github.com/mjordan/islandora_workbench/issues/1055).
* April 8, 2026: Resolved (commit 842c2f1) [issue 1072](https://github.com/mjordan/islandora_workbench/issues/1072).
* April 10, 2026: Resolved (commit 0675090) [issue 1076](https://github.com/mjordan/islandora_workbench/issues/1076).
* April 26, 2026: Resolved (commit b243fe3) [issue 1084](https://github.com/mjordan/islandora_workbench/issues/1084), (commit 017e915) [issue 1085](https://github.com/mjordan/islandora_workbench/issues/1085), and (commit e0a1d08) [issue 1086](https://github.com/mjordan/islandora_workbench/issues/1086).
* April 29, 2026: Resolved (commit 801a0c5) [issue 1087](https://github.com/mjordan/islandora_workbench/issues/1087).
* April 30, 2026: Resolved (commit 8536fc4) [issue 1082](https://github.com/mjordan/islandora_workbench/issues/1082).
* May 4, 2026: Resolved (commit c4a0673) [issue 1083](https://github.com/mjordan/islandora_workbench/issues/1083).

### Documentation

* February 4, 2026: Created a new changelog for 2026; updated note on "[Deleting media](/islandora_workbench_docs/deleting_media/)" about not being able to delete files not created by the current user; merged first draft of [running Workbench in a venv](/islandora_workbench_docs/installation_with_venv/). Thanks @ysuarez!
* February 6, 2026: Added notes to "[Configuration](https://mjordan.github.io/islandora_workbench_docs/configuration/)" and "[Optimizing your use of Workbench](https://mjordan.github.io/islandora_workbench_docs/optimizing_your_use_of_workbench/)" about benefits of using configuration file templates; added new documentation to "[User management](https://mjordan.github.io/islandora_workbench_docs/user_management/)" describing how to avoid using administrator-role users in Workbench config files.
* February 12, 2026: Added documentation on "[Using a non-administrator user](/islandora_workbench_docs/user_management/#using-a-non-administrator-user)"; added a new "[Creating nodes](/islandora_workbench_docs/creating_nodes/)" page and updated "[Choosing a task](/islandora_workbench_docs/choosing_a_task/)" to point to it; edited text on a few other pages for clarity and completeness.
* February 17, 2026: Added mention to "[Updating nodes](/islandora_workbench_docs/updating_nodes/)" and "[Updating taxonomy terms](/islandora_workbench_docs/updating_terms/)" that Paragraph fields can be updated.
* April 8, 2026: Updated "[Hooks](/islandora_workbench_docs/hooks/)" to clarify several points and examples and to note that these scripts now get a third argument, the absolute path to the Workbench config file that was specified in Workbench's --config argument; fixed a minor typo on "[Creating paged, compound, and collection content](/islandora_workbench_docs/paged_and_compound/)"; added a note to "[Troubleshooting](/islandora_workbench_docs/troubleshooting/)" about eliminating changes to firewall, proxy, and load balancer configuration before reporting a problem or requesting help.
* April 10, 2026: Added docs to "[Configuration](/islandora_workbench_docs/configuration/)" and "[Hooks](/islandora_workbench_docs/hooks/)" about the new `show_bootstrap_script_output` and `show_shutdown_script_output` settings.
* April 15, 2026: Corrected "[Assigning URL aliases](/islandora_workbench_docs/aliases/)" to clarify that nodes can have multiple URL aliases. Thanks to @dara2 for finding this error.
* April 26, 2026: Updated "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories_)" to describe how to use the new `paged_content_page_weight_multiplier` configuration setting; updated "[CSV preprocessor scripts](/islandora_workbench_docs/hooks/#csv-preprocessor-scripts)" to say that they are applied to pages/children created using the `paged_content_from_directories: true` configuration setting.
* April 29, 2026: Updated "[User managerment](/islandora_workbench_docs/user_management/)" to include docs on how to encrypt the credentials file; updated "[Known limitations](/islandora_workbench_docs/limitations/)" to remove entry about double prompt for password.
* May 1, 2026: Updated "[Ignoring CSV rows and columns](/islandora_workbench_docs/ignoring_csv_rows_and_columns/)" to document the new `csv_start_row_skip` and `csv_stop_row_skip` settings; added note about including `standalone_media_url: true` in paged documenting media-related tasks.
* May 4, 2026: Updated the "[CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/)" documentation to include the new ability to merge two databases.