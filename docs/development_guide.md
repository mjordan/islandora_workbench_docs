This documentation is aimed at developers who want to contribute to Islandora Workbench.

## General

Bug reports, improvements, feature requests, and PRs welcome. Before you open a pull request, please open an issue.

If you open a PR, please check your code with pycodestyle:

`pycodestyle --show-source --show-pep8 --ignore=E402,W504 --max-line-length=200 .`

You can also check a specific file by replacing the `.` with the filename.

Also provide tests where applicable. Tests in Workbench fall into two categories:

* Unit tests that do not require a live Islandora instance.
    * Unit tests in `tests/unit_tests.py` (run with `python3 tests/unit_tests.py`)
    * Unit tests for Workbench's Drupal fields handlers in `tests/field_tests.py` (run with `python3 tests/field_tests.py`)
* Integration tests that require a live Islandora instance running at `http://localhost:8000`
    * `tests/islandora_tests.py`, `tests/islandora_tests_check.py`,  `tests/islandora_tests_hooks.py`, and `tests/islandora_tests_paged_content.py` can be run with `python3 tests/islandora_tests.py`, etc.
    * The [Islandora Playbook](https://github.com/Islandora-Devops/islandora-playbook) is recommended way to deploy the Islandora used in these tests. Note that if an Islandora integration test fails, nodes and taxonomy terms created by the test before it fails may not be removed from Islandora.
    * Some integration and field tests output text that beings with "Error:." This is normal, it's the text that Workbench outputs when it finds something wrong (which is probably what the test is testing). Successful test (whether they test for success or failure) runs will exit with "OK". If you can figure out how to suppress this output, please visit [this issue](https://github.com/mjordan/islandora_workbench/issues/160).
* If you want to run the tests within a specific class, include the class name as an argument like this: `python3 tests/unit_tests.py TestCompareStings`


## Adding a new Drupal field type

Eventually, handlers for new Drupal field types will need to be added to Workbench. Currently, the only types supported are:

* strings (for string or text fields) like `Using Islandora Workbench for Fun and Profit`
* integers (for `field_weight`, for example) like `1` or `7281`
* the binary values `1` or `0`
* Existing Drupal-generated entity IDs (term IDs for taxonomy terms or node IDs for collections and parents), which are integers like `10` or `3549`
* structured strings, for typed relation (e.g., `relators:art:30`), link fields (e.g., `https://acme.net%%Acme Products`), geolocation fields (e.g., `"49.16667,-123.93333"`), and athority link data (e.g., `viaf%%http://viaf.org/viaf/10646807%%VIAF Record`)

Details on how to add new field types are coming soon!

## Writing tests

Also coming soon!

