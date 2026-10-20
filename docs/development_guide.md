This documentation is aimed at developers who want to contribute to Islandora Workbench.

## General

* Bug reports, improvements, feature requests, and PRs are welcome. Before you open a pull request, *please open an issue so we can discuss your idea*. In cases where the PR introduces a potentially disruptive change to Workbench, we usually want to start a discussion about its impact on users in the `#islandoraworkbench` Slack channel.
    * When you open a PR, you will be asked to complete the Workbench [PR template](https://github.com/mjordan/islandora_workbench/blob/main/.github/PULL_REQUEST_TEMPLATE.md).
* All code must be formatted using [Black](https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html). You can automatically style your code [using Black in your IDE of choice](https://black.readthedocs.io/en/stable/integrations/editors.html) or from your command line. Lint checks using Black are performed during Github CI so unlinted code will cause pull request merges to fail.
* Where applicable, unit and integration tests to accompany your code are very appreciated. Tests in Workbench fall into two categories:
    * *Unit tests* that do not require a live Islandora instance.
    * *Integration tests* that require a live Islandora instance running at `https://islandora.dev` (or some other local URL).
* `workbench_utils.py` provides a lot of utility functions or functions used across the application. Before writing a new utility function, please ensure that something similar to what you want to write doesn't already exist.

## Running tests

While developing code for Islandora Workbench, you should run tests frequently to ensure that your code hasn't introduced any regression errors. Workbench is a fairly large and complex application, and has many configuration settings. Even minor changes in the code can break things.

### Unit tests that do not require a live Islandora instance

There are several test class files containing tests that do not require a live Islandora instance to run:

* Unit tests in `tests/unit` (run with `pytest -v --no-header tests/unit`)

These tests are run automatically as GitHub actions when you push to the Islandora Workbench repo or when a merge request is merged.

### Integration tests that require a live Islandora instance

Integration tests are located in `tests/integration` and require an Islandora instance with the `islandora_workbench_integration` module running at either `https://islandora.io` or the hostname (and scheme) set in the environment variable `WB_INTEGRATION_TEST_HOST`.

The tests are expecting an administrator with username `admin` and password `admin`, as well as a non-admin user with username `test-user` and password `testPassword`. If those configuration settings match your local Islandora configuration (e.g. running in Docker), you can run these tests without building a set of local test files. If your local setup requires different settings, you can run the tests after building a set of local tests (explained below).

Assuming you have an Islandora instance running you can run the integration tests in `tests/integration` with `pytest -v --no-header tests/integration`.

#### Environment variables

If you have a working Islandora to test against but it is not located at `https://islandora.io` then you can pass the hostname by setting the `WB_INTEGRATION_TEST_HOST` environment variable.

For example.
```bash
> WB_INTEGRATION_TEST_HOST=http://my-islandora.host pytest -v --no-header tests/integration
```

### Running individual tests

If you want to run a single file of tests, single class of tests or a single test method you can by providing the filename and/or the method name with the format `pytest -v <path to test file>::<test class name>::<test function name>`

For example you can run an entire file of tests (example output truncated)
```bash
> pytest -v --no-header tests/unit/field_test.py 
=============================================================== test session starts ================================================================
collected 105 items                                                                                                                                

tests/unit/field_test.py::TestSimpleField::test_create_with_simple_field PASSED                                                              [  0%]
tests/unit/field_test.py::TestSimpleField::test_create_with_simple_float_field PASSED                                                        [  1%]
tests/unit/field_test.py::TestSimpleField::test_create_with_simple_integer_field PASSED                                                      [  2%]
tests/unit/field_test.py::TestSimpleField::test_simple_field_dudupe_values PASSED                                                            [  3%]
tests/unit/field_test.py::TestSimpleFieldFormatted::test_create_with_simple_field PASSED                                                     [ 18%]
tests/unit/field_test.py::TestSimpleFieldFormatted::test_simple_field_dudupe_values PASSED                                                   [ 19%]

=============================================================== 105 passed in 4.48s ================================================================
```

Or a single class in a file
```bash
> pytest -v --no-header tests/unit/field_test.py::TestSimpleField
=============================================================== test session starts ================================================================
collected 18 items                                                                                                                                 

tests/unit/field_test.py::TestSimpleField::test_create_with_simple_field PASSED                                                              [  5%]
tests/unit/field_test.py::TestSimpleField::test_create_with_simple_float_field PASSED                                                        [ 11%]
tests/unit/field_test.py::TestSimpleField::test_create_with_simple_integer_field PASSED                                                      [ 16%]
tests/unit/field_test.py::TestSimpleField::test_simple_field_dudupe_values PASSED                                                            [ 22%]

================================================================ 18 passed in 0.97s ================================================================
```

or a single test
```bash
> pytest -v --no-header tests/unit/field_test.py::TestSimpleField::test_create_with_simple_field
=============================================================== test session starts ================================================================
collected 1 item                                                                                                                                   

tests/unit/field_test.py::TestSimpleField::test_create_with_simple_field PASSED                                                              [100%]

================================================================ 1 passed in 0.98s =================================================================
```

## Writing tests

Islandora Workbench's tests are written using a mixture of the Python built-in [unittest](https://docs.python.org/3/library/unittest.html) library and the newer [pytest](https://docs.pytest.org/en/stable/) (version 8+), and as explained above, fall into two categories:

- Unit tests that do not require a live Islandora instance.
- Integration tests that require a live Islandora instance running the islandora_workbench_integration module.

`pytest` accepts any class to run tests in. It also allows for complex set ups with "parameterized" tests and the use of "fixtures". This allows the same test function to be run multiple times with different inputs. All tests must have a function name that begins with `test` (i.e. `test_new_inputs`, `test_my_feature`, etc) to be considered a test. A "fixture" is a function in a test class that allows you to provide inputs to your test. See https://docs.pytest.org/en/stable/how-to/fixtures.html

There is a provided base class for integration tests which defines parameters and provides the setup. This base class is in `tests/workbench_base_class.py`

Always add new tests to Workbench's `tests` directory, not `tests_local`. `tests` is used by Github's CI runners, so new tests must be present in that directory during merge requests. Since running `scripts/make_local_tests.py` copies everthing in `tests` to `tests_local`, any changes you make (or additions) to files in `tests` will be copied over to `tests_local` intact.

### A simple unit test

Islandora Workbench unit tests are much like unit tests in any Python application. The sample test below, from `tests/unit_test.py`, tests the `validate_latlong_value()` method from the `workbench_utils.py` module. Since `workbench_utils.validate_latlong_value()` doesn't interact with Islandora, `https://islandora.traefik.me/` doesn't need to be available to run this unit test.

```python
class TestValidateLatlongValue(unittest.TestCase):

    def test_validate_good_latlong_values(self):
        values = ['+90.0, -127.554334', '90.0, -127.554334', '-90,-180', '+50.25,-117.8', '+48.43333,-123.36667']
        for value in values:
            res = workbench_utils.validate_latlong_value(value)
            self.assertTrue(res)

    def test_validate_bad_latlong_values(self):
        values = ['+90.1 -100.111', '045, 180', '+5025,-117.8', '-123.36667']
        for value in values:
            res = workbench_utils.validate_latlong_value(value)
            self.assertFalse(res)
```

The same test can be written as 

```python
class TestValidateLatlongValue:

    def test_validate_good_latlong_values(self):
        values = ['+90.0, -127.554334', '90.0, -127.554334', '-90,-180', '+50.25,-117.8', '+48.43333,-123.36667']
        for value in values:
            res = workbench_utils.validate_latlong_value(value)
            assert res is True

    def test_validate_bad_latlong_values(self):
        values = ['+90.1 -100.111', '045, 180', '+5025,-117.8', '-123.36667']
        for value in values:
            res = workbench_utils.validate_latlong_value(value)
            assert res is False
```

now that we run with `pytest`

This is a fairly standard Python unit test - we define a list of valid lat/long pairs and run them through the `workbench_utils.validate_latlong_value()` method expecting it to return `True` for each value, and then we define a list of bad lat/long pairs and run them through the method expecting it to return `False` for each value.

### A simple integration test

The two sample integration tests provided below are copied from `islandora_test.py`.

The first sample integration test, `TestCreate`, looks like this (with line numbers added for easy reference). Configuration and CSV files used by this test are in `tests/assets/create_test`:

```python
1.  class TestCreate(WorkbenchTest):
2.    # WorkbenchTest is the base class located in tests/workbench_test_class.py.
3.  
4.      def test_create(self, workbench_user):
5.        # workbench_user is the parameter which provides an Administrator and then a regular user with use_islandora_workbench permission.
6.          configuration = {
7.              "task": "create",
8.              "host": WB_INTEGRATION_TEST_HOST,
9.              "input_dir": "tests/assets/create_test",
10.             "media_type": "image",
11.             "allow_missing_files": True,
12.             "secure_ssl_only": False,
13.         }
14.         #  Provide the configuration as a dict in each test.
15.         configuration = workbench_user.alter_configuration(configuration)
16.         # This alters the configuration dictionary to add the credentials information.
17.         create_config_file_path = self.write_config_and_get_path(configuration)
18.         # This writes out the configuration to a Yaml file and returns the path to that file.
19.
20.         create_cmd = ["./workbench", "--config", create_config_file_path]
21.         create_output = subprocess.check_output(create_cmd, cwd=self.workbench_dir)
22.         create_output = create_output.decode().strip()
23.         nids = collect_nids_from_create_output(create_output)
24.         # Run the workbench instance and check the output.
25.
26.         try:
27.             assert len(nids) == 2
28.         finally:
29.           # Use finally to ensure the below clean up of created nodes occurs even if the test fails.
30.             for nid in nids:
31.                 quick_delete_cmd = [
32.                     "./workbench",
33.                     "--config",
34.                     create_config_file_path,
35.                     "--quick_delete_node",
36.                     f"{configuration['host']}/node/{nid}",
37.                 ]
38.                 subprocess.check_output(quick_delete_cmd, cwd=self.workbench_dir)
39.
40.             cleanup_paths(
41.                 create_config_file_path,
42.                 os.path.join(
43.                     self.tests_dir,
44.                     "assets",
45.                     "create_test",
46.                     "metadata.csv.preprocessed",
47.                 ),
48.                 os.path.join(self.tests_dir, "assets", "create_test", "rollback.csv"),
49.             )
50.             # cleanup_paths is a helper function to delete any files or directories provided as arguments.
```
The test runs Workbench using Python's `subprocess.check_output()` method, grabs the node IDs from the output from the "created at" strings emitted by Workbench (using the helper function (line 23) `collect_nids_from_create_output(output_line: str) -> list`) and then counts the number of members in that list. If the number of nodes created matches the expected number, the test passes.

Since this test creates some nodes, we then put the target Drupal back into as close a state as we started with as possible. After the test assertions are checked the list of node IDs created in `test_create()` are used to run Workbench with the `--quick_delete_node` option. It then removes any temporary files created during the test.

### A more complex integration test

Since Workbench is essentially a specialized REST client, writing integration tests that require interaction with Drupal can get a bit complex. But, the overall pattern is:

1. Create some entities (nodes, media, taxonomy terms).
1. Confirm that they were created in the expected way (doing this usually involves keeping track of any node IDs needed to run tests or to clean up, and in some cases involves parsing out values from raw JSON returned by Drupal).
1. Clean up by deleting any Drupal entities created during the tests and also any temporary local files.

An integration test that checks data in the node JSON is `TestUpdateWithMaxNodeTitleLength()`. Files that accompany this test are in `tests/assets/max_node_title_length_test`. Here is a copy of the test's code:


```python
1.  class TestUpdateWithMaxNodeTitleLength(WorkbenchTest):
2.
3.      @pytest.fixture
4.      def setup_nodes(self, workbench_user):
5.          requests.packages.urllib3.disable_warnings()
6. 
7.          configuration = {
8.              "task": "create",
9.              "host": WB_INTEGRATION_TEST_HOST,
10.             "input_dir": "tests/assets/max_node_title_length_test",
11.             "input_csv": "create_max_node_title_length.csv",
12.             "nodes_only": True,
13.             "max_node_title_length": 30,
14.             "secure_ssl_only": False,
15.         }
16.         configuration = workbench_user.alter_configuration(configuration)
17.         create_config_file_path = self.write_config_and_get_path(configuration)
18.         create_cmd = ["./workbench", "--config", create_config_file_path]
19.
20.         create_output = subprocess.check_output(create_cmd, cwd=self.workbench_dir)
21.         create_output = create_output.decode().strip()
22.         nids = collect_nids_from_create_output(create_output)
23.
24.         assert len(nids) == 6
25.
26.         # Write out an update CSV file using the node IDs in self.nids.
27.         test_titles = [
28.             "This title is 37 chars___________long",
29.             "This title is 39 chars_____________long",
30.             "This title is 29 _ chars long",
31.             "This title is 42 chars________________long",
32.             "This title is 44 chars__________________long",
33.             "This title is 28 chars long.",
34.         ]
35.
36.         with tempfile.NamedTemporaryFile(
37.             mode="wt", delete=False, suffix="_update_max_node_title_length.csv"
38.         ) as update_csv_file:
39.             update_csv_file.write("node_id,title\n")
40.             for i in range(0, 5):
41.                 update_csv_file.write(f"{nids[i]},{test_titles[i]}\n")
42.             update_csv_file_name = update_csv_file.name
43.
44.         # This is where the test runs from.
45.         yield {
46.             "update_csv_file_path": update_csv_file_name,
47.             "nids": nids,
48.             "workbench_user": workbench_user,
49.         }
50.
51.         for nid in nids:
52.             quick_delete_cmd = [
53.                 "./workbench",
54.                 "--config",
55.                 create_config_file_path,
56.                 "--quick_delete_node",
57.                 f"{configuration['host']}/node/{nid}",
58.             ]
59.             subprocess.check_output(quick_delete_cmd)
60.
61.         cleanup_paths(
62.             update_csv_file_name,
63.             os.path.join(
64.                 self.tests_dir,
65.                 "assets",
66.                 "max_node_title_length_test",
67.                 "rollback.csv",
68.             ),
69.             os.path.join(
70.                 self.temp_dir, "create_max_node_title_length.csv.preprocessed"
71.             ),
72.             create_config_file_path,
73.         )
74.
75.     def test_update(self, setup_nodes):
76.         configuration = {
77.             "task": "update",
78.             "host": WB_INTEGRATION_TEST_HOST,
79.             "input_dir": "tests/assets/max_node_title_length_test",
80.             "input_csv": setup_nodes["update_csv_file_path"],
81.             "max_node_title_length": 30,
82.             "secure_ssl_only": False,
83.         }
84.         configuration = setup_nodes["workbench_user"].alter_configuration(configuration)
85.         update_config_file_path = self.write_config_and_get_path(configuration)
86.         update_cmd = ["./workbench", "--config", update_config_file_path]
87.         # Run the update command.
88.         subprocess.check_output(update_cmd, cwd=self.workbench_dir)
89.
90.         # Fetch each node created in setup_nodes and check to see if its title is <= 30 chars long. All should be.
91.         try:
92.             for nid_to_update in setup_nodes["nids"]:
93.                 node_url = (
94.                     f"{WB_INTEGRATION_TEST_HOST}/node/"
95.                     + str(nid_to_update)
96.                     + "?_format=json"
97.                 )
98.                 node_response = requests.get(node_url, verify=False)
99.                 assert node_response.status_code == 200
100.                node = json.loads(node_response.text)
101.                updated_title = str(node["title"][0]["value"])
102.                assert (
103.                    len(updated_title) <= 30
104.                ), f"Node {nid_to_update} title is longer than 30 characters: {updated_title}"
105.        finally:
106.            # delete the update .preprocessed file and config file.
107.            cleanup_paths(
108.                os.path.join(
109.                    self.temp_dir, "update_max_node_title_length.csv.preprocessed"
110.                ),
111.                update_config_file_path,
112.            )
```

This test:

1. (lines 3-4) Defines a fixture to do some setup, that fixture calls another fixture (`workbench_user`) first.
1. (lines 7-24) Creates some new nodes and ensures the expected number were created.
1. (lines 27-42) Creates a new file in the temporary directory with titles for each of the created node IDs.
1. (lines 45-49) The fixture yields this dictionary of data to the test that calls it, this fixture will resume running here once the test function is complete.
1. (lines 51-59) [**after the test runs**] Run workbench with the `--quick_delete_node` option to remove the nodes created by this test.
1. (lines 61-73) [**after the test runs**] Delete any generated files to clean up the filesystem.
1. (line 75) Define the test, which has an argument of the fixture's (line 3) name. This tells it to find and call that function.
1. (lines 76-83) Define the configuration which uses data from the fixture by referencing the dictionary yielded on line 45.
1. (line 84) Because we passed the `workbench_user` fixture into the `setup_nodes` fixture, but still need to access it in the test we include it in the yield 
1. (line 88) Run the update command.
1. (lines 92-104) Fetch and check that each node title was (or was not) updated correctly.
1. (lines 107-112) Clean up the file paths specific to the test and end (passing control back to the fixture `setup_nodes` to continue its run).


## Adding a new Drupal field type

#### Overview of how Workbench handles field types

Workbench and Drupal exchange field data represented as JSON, via Drupal's REST API. The specific JSON structure depends on the field type (text, entity reference, geolocation, etc.). Handling the details of these structures when adding new field data during `create` tasks, updating existing field data during `update` tasks, etc. is performed by code in the `workbench_fields.py` module.

Each distinct field structure has its own class in that file, but they are all subclasses of the `WorkbenchField` base class which has the methods `create()`, `update()`, `dedupe_values()`, `remove_invalid_values()`, and `serialize()`. The `create()` and `update()` methods convert CSV field data in Workbench input files to Python dictionaries, which are subsequently converted into JSON for pushing up to Drupal. The `serialize()` method reverses this conversion, taking the JSON field data fetched from Drupal and converting it into a dictionary, and from that, into CSV data. `dedupe_values()` and `remove_invalid_values()` are utility methods that do what their names suggest.

There are default behaviours provided for the `create()` and `dedupe_values()` methods. The rest raise an Exception if you do not override them with specific behaviour for your field type.

Currently, Workbench supports the following field types:

* "simple" fields for
    * strings (for string or text fields)
    * integers
    * binary values 1 and 0
    * existing Drupal-generated entity IDs
* entity reference fields
* entity reference revision fields
* typed relation fields
* link fields
* geolocation fields
* authority link fields
* media track fields
* linked data fields

Eventually, classes for new Drupal field types will need to be added to Workbench as the community adopts more field types provided by Drupal contrib modules or creates new field types specific to Islandora.

All `WorkbenchField` fields also need to be added to the `WorkbenchFieldFactory`'s `field_to_class_map`. This allows a user anywhere in the code base to generate a field object using the field identifier from Drupal (i.e `new_field = WorkbenchFieldFactory.get_field_handler(field_type)`). The `WorkbenchFieldFactory` is located at the bottom of the `workbench_fields.py` file.

#### Field class methods

The most complex aspect of handling field data is cardinality, or in other words, whether a given field's configuration setting "Allowed number of values" allows for a single value, a maximum number of values (for example, a maximum of 3 values), or an unlimited number of values.

![Maximum field values example](images/max_field_values.png)

Each field type's cardinality configuration complicates creating and updating field instances because Workbench must deal with situations where input CSV data for a field contains more values than are allowed, or when the user wants to append a value to an existing field instance rather than replace existing values, potentially exceeding the field's configured cardinality. Drupal's REST interface is very strict about cardinality, and if Workbench tries to push up a field's JSON that violates the field's cardinality, the HTTP request fails and returns a `422 Unprocessable Content` response for the node containing the malformed field data.

To prevent this from happening, code within field type classes needs to contain logic to account for the three different types of cardinality (cardinality of 1, cardinality of more than 1 but with a maximum, and unlimited) and for the specific JSON structure created from the field data in the input CSV. When Workbench detects that the number of field instances in the CSV data surpasses the field's maximum configured cardinality, it will truncate the incoming data and log that it did so via the `log_field_cardinality_violation()` utility function. However, truncating data is a fallback behavior; `--check` will report intances of this cardinality violation, providing you with an opportunity to correct the CSV data before committing it to Drupal.

To illustrate this complexity, let's look at the `update()` method within the `SimpleField` class, which handles field types that have the Python structure `[{"value": value}]` or, for "formatted" text, `[{"value": value, "format": text_format}]`.

!!! note
    Note that the example structure in the preceding paragraph shows a single value for that field. It's a list, but a list containing a single dictionary. If there were two values in a field, the structure would be a list containing two dictionaries, like `[{"value": value}, {"value": value}]`. If the field contained three values, the structure would be `[{"value": value}, {"value": value}, {"value": value}]`

Lines 57-93 apply if the `update_mode` configuration setting is "append", and lines 95-130 apply if the `update_mode` setting is "replace". If the cardinality is limited (i.e. 0 to some number) and the number of values is larger than the cardinality, the values are truncated to the cardinality setting. In an **append** `update_mode`  the values are appended then the list is truncated. In a **replace** `update_mode` the values are truncated then values are appended. An `update_mode` setting of "delete" simply removes all values from the field, in lines 33-35.

```python
1.  def update(
2.      self,
3.      config: dict,
4.      field_definitions: dict,
5.      entity: dict,
6.      row: OrderedDict,
7.      field_name: str,
8.      entity_field_values: list,
9.  ) -> dict:
10.     """Note: this method appends incoming CSV values to existing values, replaces existing field
11.     values with incoming values, or deletes all values from fields, depending on whether
12.     config['update_mode'] is 'append', 'replace', or 'delete'. It doesn not replace individual
13.     values within fields.
14.     Parameters
15.       ----------
16.         config : dict
17.             The configuration settings defined by workbench_config.get_config().
18.         field_definitions : dict
19.             The field definitions object defined by get_field_definitions().
20.         entity : dict
21.             The dict that will be POSTed to Drupal as JSON.
22.         row : OrderedDict.
23.             The current CSV record.
24.         field_name : string
25.             The Drupal fieldname/CSV column header.
26.         entity_field_values : list
27.             List of dictionaries containing existing value(s) for field_name in the entity being updated.
28.         Returns
29.         -------
30.         dictionary
31.             A dictionary representing the entity that is PATCHed to Drupal as JSON.
32.     """
33.     if config["update_mode"] == "delete":
34.         entity[field_name] = []
35.         return entity
36.
37.     if not row[field_name]:
38.         return entity
39.
40.     if field_name not in entity:
41.         entity[field_name] = []
42.
43.     if field_name in config["field_text_format_ids"]:
44.         text_format = config["field_text_format_ids"][field_name]
45.     else:
46.         text_format = config["text_format_id"]
47.
48.     if config["task"] == "update_terms":
49.         entity_id_field = "term_id"
50.     elif config["task"] == "update":
51.         entity_id_field = "node_id"
52.     elif config["task"] in ["update_media", "update_media_by_node"]:
53.         entity_id_field = "media_id"
54.
55.     cardinality = int(field_definitions[field_name].get("cardinality", -1))
56.
57.     if config["update_mode"] == "append":
58.         subvalues = str(row[field_name]).split(config["subdelimiter"])
59.            subvalues = self.dedupe_values(subvalues)
60.         subvalues = self.remove_invalid_values(
61.             config, field_definitions, field_name, subvalues
62.         )
63.         for subvalue in subvalues:
64.             subvalue = truncate_csv_value(
65.                 field_name,
66.                 row[entity_id_field],
67.                 field_definitions[field_name],
68.                 subvalue,
69.             )
70.             if (
71.                 "formatted_text" in field_definitions[field_name]
72.                 and field_definitions[field_name]["formatted_text"] is True
73.             ):
74.                 entity_field_values.append(
75.                     {"value": subvalue, "format": text_format}
76.                 )
77.             else:
78.                 if field_definitions[field_name][
79.                     "field_type"
80.                 ] == "integer" and value_is_numeric(subvalue):
81.                     subvalue = int(subvalue)
82.                 if field_definitions[field_name][
83.                     "field_type"
84.                 ] == "float" and value_is_numeric(subvalue, allow_decimals=True):
85.                     subvalue = float(subvalue)
86.                 entity_field_values.append({"value": subvalue})
87.         entity_field_values = self.dedupe_values(entity_field_values)
88.         if -1 < cardinality < len(entity_field_values):
89.             log_field_cardinality_violation(
90.                 field_name, row[entity_id_field], str(cardinality)
91.             )
92.             entity_field_values = entity_field_values[:cardinality]
93.         entity[field_name] = entity_field_values
94.
95.     elif config["update_mode"] == "replace":
96.         field_values = []
97.         subvalues = str(row[field_name]).split(config["subdelimiter"])
98.         subvalues = self.remove_invalid_values(
99.             config, field_definitions, field_name, subvalues
100.        )
101.        subvalues = self.dedupe_values(subvalues)
102.        if -1 < cardinality < len(subvalues):
103.            log_field_cardinality_violation(
104.                field_name, row[entity_id_field], str(cardinality)
105.            )
106.            subvalues = subvalues[:cardinality]
107.        for subvalue in subvalues:
108.            subvalue = truncate_csv_value(
109.                field_name,
110.                row[entity_id_field],
111.                field_definitions[field_name],
112.                subvalue,
113.            )
114.            if (
115.                "formatted_text" in field_definitions[field_name]
116.                and field_definitions[field_name]["formatted_text"] is True
117.            ):
118.                field_values.append({"value": subvalue, "format": text_format})
119.            else:
120.                if field_definitions[field_name][
121.                    "field_type"
122.                ] == "integer" and value_is_numeric(subvalue):
123.                    subvalue = int(subvalue)
124.                if field_definitions[field_name][
125.                    "field_type"
126.                ] == "float" and value_is_numeric(subvalue, allow_decimals=True):
127.                    subvalue = float(subvalue)
128.                field_values.append({"value": subvalue})
129.        field_values = self.dedupe_values(field_values)
130.        entity[field_name] = field_values
131.
132.    return entity
```

Each field type has its own structure. Within the field classes, the field structure is represented in Python dictionaries and converted to JSON when pushed up to Drupal as part of REST requests. These Python dictionaries are converted to JSON automatically as part of the HTTP request to Drupal (you do not do this within the field classes) so we'll focus only on the Python dictionary structure here. Some examples are:

* `SimpleField` fields have the Python structure `{"value": value}` or, for "formatted" text, `{"value": value, "format": text_format}`
* `GeolocationField` fields have the Python structure `{"lat": lat_value, "lon": lon_value}`
* `LinkField` fields have the python structure `{"uri": uri, "title": title}`
* `EntityReferenceField` fields have the Python structure `{"target_id": id, "target_type": target_type}`
* `TypedRelationField` fields have the Python structure `{"target_id": id, "rel_type": rel_type:rel_value, "target_type": target_type}`
    * the value of the `rel_type` key is the relator type (e.g. MARC relators) and the relator value (e.g. 'art') joined with a colon, e.g. `relators:art`
* `AuthorityLinkField` fields have the Python structure `{"source": source, "uri": uri, "title": title}`
* `MediaTrackField` fields have the Python structure `{"label": label, "kind": kind, "srclang": lang, "file_path": path}`
* `LinkedDataField` fields have the Python structure `{"url": uri, "value": label}`

To add a support for a new field type, you will need to figure out the field type's JSON structure and convert that structure into the Python dictionary equivalent in the new field class methods. The best way to inspect a field type's JSON structure is to view the JSON representation of a node that contains instances of the field by tacking `?_format=json` to the end of the node's URL. Once you have an example of the field type's JSON, you will need to write the necessary class methods to convert between Workbench CSV data and the field's JSON structure as applicable in all of the field class methods, making sure you account for the field's configured cardinality, account for the update mode within the `update()` method, etc.

Writing field classes is one aspect of Workbench development that demonstrates the value of unit tests. Without writing unit tests to accompany the development of these field classes, you *will* lose your mind. [tests/units/field_test.py](https://github.com/mjordan/islandora_workbench/blob/main/test/unit/field_test.py) contains over 80 tests in more than 5,000 lines of test code for a good reason.

## Islandora Workbench Integration Drupal module

[Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) is a Drupal module that allows Islandora Workbench to communicate with Drupal efficiently and reliably. It enables some Views and REST endpoints that Workbench expects, and also provides a few custom REST endpoints (see the module's README for details).

Generally speaking, the most common reason the Integration module will need to be updated (apart from requirements imposed by new versions of Drupal) is if we add a new feature to Workbench that requires a specific View or a specific REST endpoint to be enabled and configured in the target Drupal. If a change is required in the Integration module, it is very important to communicate this to Workbench users, since if the Integration module is not updated to align with the change in Workbench, the new feature won't work.

To mitigate against the possibility that the version of the Integration module is not recent enough to support specific features in Workbench, every time it is run, Workbench uses the `check_integration_module_version()` function to check the Integration module's version number and then to use conditional logic to determine if the module's version number meets or exceeds a version number string defined in that function.

As a developer, you need to add logic to the block of code within `check_integration_module_version()` starting with the comment `Logic that maps a feature to a miniumum Integration module version, and produces a specific log message.` that defines the minimum major and minor release version (in semantic version terms) that your Workbench code requires, and to create a suitable log message explaining the requirement.

!!! note
    Some Views used by Islandora Workbench are defined by users and not by the Islandora Workbench Integration module. Specifically, Views described in the "[Generating CSV files](https://mjordan.github.io/islandora_workbench_docs/generating_csv_files/)" documentation are created by users.
