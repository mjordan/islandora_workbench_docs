This documentation is aimed at developers who want to contribute to Islandora Workbench.

## General

Bug reports, improvements, feature requests, and PRs welcome. Before you open a pull request, please open an issue.

If you open a PR, please check your code with pycodestyle:

`pycodestyle --show-source --show-pep8 --ignore=E402,W504 --max-line-length=200 .`

You can also check a specific file by replacing the `.` with the filename.

Also provide tests where applicable (see "Writing tests", below). Tests in Workbench fall into two categories:

1. Unit tests that do not require a live Islandora instance.
    * Unit tests in `tests/unit_tests.py` (run with `python3 tests/unit_tests.py`)
    * Unit tests for Workbench's Drupal fields handlers in `tests/field_tests.py` (run with `python3 tests/field_tests.py`)
1. Integration tests that require a live Islandora instance running at `https://islandora.traefik.me/`
    * `tests/islandora_tests.py`, `tests/islandora_tests_check.py`,  `tests/islandora_tests_hooks.py`, and `tests/islandora_tests_paged_content.py` can be run with `python3 tests/islandora_tests.py`, etc.
    * The Islandora Starter Site deployed with [ISLE](https://github.com/Islandora-Devops/isle-dc) is recommended way to deploy the Islandora used in these tests. Integration tests remove all nodes and media added during the tests, unless a test fails. Taxonomy terms created by tests are not removed.
    * Some integration and field tests output text that beings with "Error:." This is normal, it's the text that Workbench outputs when it finds something wrong (which is probably what the test is testing). Successful test (whether they test for success or failure) runs will exit with "OK". If you can figure out how to suppress this output, please visit [this issue](https://github.com/mjordan/islandora_workbench/issues/160).

If you want to run the tests within a specific class, include the class name as an argument like this: `python3 tests/unit_tests.py TestCompareStings`


## Adding a new Drupal field type

Eventually, handlers for new Drupal field types will need to be added to Workbench as the community adopts more field types provided by Drupal contrib modules or creates new field types specific to Islandora. Currently, Workbench supports the following field types:

* "simple" fields for
    * strings (for string or text fields) like `Using Islandora Workbench for Fun and Profit`
    * integers (for `field_weight`, for example) like `1` or `7281`
    * the binary values `1` or `0`
    * existing Drupal-generated entity IDs (term IDs for taxonomy terms or node IDs for collections and parents), which are integers like `10` or `3549`
* entity reference fields
* entity reference revision fields
* typed relation fields (e.g., `relators:art:30`)
* link fields (e.g., `https://acme.net%%Acme Products`)
* geolocation fields (e.g., `"49.16667,-123.93333"`)
* authority link fields (e.g., `viaf%%http://viaf.org/viaf/10646807%%VIAF Record`)

All field types are defined in classes contained in `workbench_fields.py` and share the methods `create()`, `update()`, `dedupe_values()`, `remove_invalid_values()`, and `serialize()`.

!!! note
    Details on how to add new field types are coming soon!

## Writing tests

Islandora Workbench's tests are written using the Python built-in [module](https://docs.python.org/3/library/unittest.html) `unittest`, and as explained above, fall into two categories:

- Unit tests that do not require a live Islandora instance.
- Integration tests that require a live Islandora instance running at `https://islandora.traefik.me/`.

The unit tests are pretty conventional, but the integration tests are a bit more challenging. The two sample tests provided below are copied from `islandora_tests.py`, and you can see their input files in `tests/assets/create_test` and `tests/assets/max_node_title_length_test`, respectively.

### A simple unit test

This test, from `tests/unit_tests.py`, tests the `validate_latlong_value()` method from the `workbench_utils.py` module.

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

This is a fairly standard Python unit test - we define a list of valid lat/long pairs and run them through the `workbench_utils.validate_latlong_value()` method expecting it to return `True` for each value, and then we define a list of bad lat/long pairs and run them through the method expecting it to return `False` for each value. Since `workbench_utils.validate_latlong_value()` doesn't interact with Islandora, `https://islandora.traefik.me/` doesn't need to be available to run this unit test.

### A simple integration test

An example of a simple integration test is `TestCreate`, whose code (in `islandora_tests.py`) looks like this (with line numbers added for easy reference):

```python
1. class TestCreate(unittest.TestCase):
2.
3.     def setUp(self):
4.         self.current_dir = os.path.dirname(os.path.abspath(__file__))
5.         self.create_config_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'create.yml')
6.         self.create_cmd = ["./workbench", "--config", self.create_config_file_path]
7.
8.     def test_create(self):
9.         self.nids = list()
10.         create_output = subprocess.check_output(self.create_cmd)
11.         create_output = create_output.decode().strip()
12.         create_lines = create_output.splitlines()
13.         for line in create_lines:
14.             if 'created at' in line:
15.                 nid = line.rsplit('/', 1)[-1]
16.                 nid = nid.strip('.')
17.                 self.nids.append(nid)
18.
19.         self.assertEqual(len(self.nids), 2)
20.
21.     def tearDown(self):
22.         for nid in self.nids:
23.             quick_delete_cmd = ["./workbench", "--config", self.create_config_file_path, '--quick_delete_node', 'https://islandora.traefik.me/node/' + nid]
24.             quick_delete_output = subprocess.check_output(quick_delete_cmd)
25.
26.         self.rollback_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'rollback.csv')
27.         if os.path.exists(self.rollback_file_path):
28.             os.remove(self.rollback_file_path)
29.
30.         self.preprocessed_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'metadata.csv.preprocessed')
31.         if os.path.exists(self.preprocessed_file_path):
32.             os.remove(self.preprocessed_file_path)

```

As you can see, this test runs Workbench using the config file `create.yml` (line 10), which lives at `tests/assets/create_test/create.yml`, relative to the workbench directory. A tricky aspect of using real config files in tests is that all paths mentioned in the config file must be relative to the workbench directory. This `create.yml` defines the `input_dir` setting to be `tests/assets/create_test`:

```yaml
task: create
host: https://islandora.traefik.me
username: admin
password: password
input_dir: "tests/assets/create_test"
media_type: image
allow_missing_files: True
```

The test's `setUp()` method prepares the file paths, etc. and within the test's only test method, `test_create()`, runs Workbench using Python's `subprocess.check_output()` method, grabs the node IDs from the output from the "created at" strings emitted by Workbench (lines 14-17), adds them to a list, and then counts the number of members in that list. If the number of nodes created matches the expected number, the test passes.

Since this test creates some nodes, we use the test class's `tearDown()` method to put the target Drupal back into as close a state as we started with as possible. `tearDown()` basically takes the list of node IDs created in `test_create()` and runs Workbench with the `--quick_delete_node` option. It then removes any temporary files created during the test.

### A more complex integration test

Since Workbench is essentially a specialized REST client, writing integration tests that require interaction with Drupal can get a bit complex. But, the overall pattern is:

1. Create some entities (nodes, media, taxonomy terms).
1. Confirm that they were created in the expected way (doing this usually involves keeping track of any node IDs needed to run tests or to clean up, and in some cases parsing out values from raw JSON returned by Drupal).
1. Clean up by deleting any Drupal entities created during the tests and also any temporary local files.

An integration test that checks data in the node JSON is `TestUpdateWithMaxNodeTitleLength()`. Here is a copy of its code:

```python
1. class TestUpdateWithMaxNodeTitleLength(unittest.TestCase):
2.
3.     def setUp(self):
4.         self.current_dir = os.path.dirname(os.path.abspath(__file__))
5.         self.create_config_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'create.yml')
6.         self.create_cmd = ["./workbench", "--config", self.create_config_file_path]
7.         self.nids = list()
8.
9.         self.update_csv_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'update_max_node_title_length.csv')
10.         self.update_config_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'update.yml')
11.         self.update_cmd = ["./workbench", "--config", self.update_config_file_path]
12.
13.         self.temp_dir = tempfile.gettempdir()
14.
15.     def test_create(self):
16.         create_output = subprocess.check_output(self.create_cmd)
17.         self.create_output = create_output.decode().strip()
18.
19.         create_lines = self.create_output.splitlines()
20.         for line in create_lines:
21.             if 'created at' in line:
22.                 nid = line.rsplit('/', 1)[-1]
23.                 nid = nid.strip('.')
24.                 self.nids.append(nid)
25.
26.         self.assertEqual(len(self.nids), 6)
27.
28.         # Write out an update CSV file using the node IDs in self.nids.
29.         update_csv_file_rows = list()
30.         test_titles = ['This title is 37 chars___________long',
31.                        'This title is 39 chars_____________long',
32.                        'This title is 29 _ chars long',
33.                        'This title is 42 chars________________long',
34.                        'This title is 44 chars__________________long',
35.                        'This title is 28 chars long.']
36.         update_csv_file_rows.append('node_id,title')
37.         i = 0
38.         while i <= 5:
39.             update_csv_file_rows.append(f'{self.nids[i]},{test_titles[i]}')
40.             i = i + 1
41.         with open(self.update_csv_file_path, mode='wt') as update_csv_file:
42.             update_csv_file.write('\n'.join(update_csv_file_rows))
43.
44.         # Run the update command.
45.         check_output = subprocess.check_output(self.update_cmd)
46.
47.         # Fetch each node in self.nids and check to see if its title is <= 30 chars long. All should be.
48.         for nid_to_update in self.nids:
49.             node_url = 'https://islandora.traefik.me/node/' + str(self.nids[0]) + '?_format=json'
50.             node_response = requests.get(node_url)
51.             node = json.loads(node_response.text)
52.             updated_title = str(node['title'][0]['value'])
53.             self.assertLessEqual(len(updated_title), 30, '')
54.
55.     def tearDown(self):
56.         for nid in self.nids:
57.             quick_delete_cmd = ["./workbench", "--config", self.create_config_file_path, '--quick_delete_node', 'https://islandora.traefik.me/node/' + nid]
58.             quick_delete_output = subprocess.check_output(quick_delete_cmd)
59.
60.         self.rollback_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'rollback.csv')
61.         if os.path.exists(self.rollback_file_path):
62.             os.remove(self.rollback_file_path)
63.
64.         self.preprocessed_file_path = os.path.join(self.temp_dir, 'create_max_node_title_length.csv.preprocessed')
65.         if os.path.exists(self.preprocessed_file_path):
66.             os.remove(self.preprocessed_file_path)
67.
68.         # Update test: 1) delete the update CSV file, 2) delete the update .preprocessed file.
69.         if os.path.exists(self.update_csv_file_path):
70.             os.remove(self.update_csv_file_path)
71.
72.         self.preprocessed_update_file_path = os.path.join(self.temp_dir, 'update_max_node_title_length.csv.preprocessed')
73.         if os.path.exists(self.preprocessed_update_file_path):
74.             os.remove(self.preprocessed_update_file_path)
```

This test:

1. (line 16) creates some nodes that will be updated within the same test class (i.e. in line 45)
1. (lines 28-42) writes out a temporary CSV file which will be used as the `input_csv` file in a subsequent `update` task containing the new node IDs plus some titles that are longer than `max_node_title_length: 30` setting in the `assets/max_node_title_length_test/update.yml` file
1. (line 45) runs `self.update_cmd` to execute the `update` task
1. (lines 47-53) fetches the title values for each of the updated nodes and tests the length of each title string to confirm that it does not exceed the maximum allowed length of 30 characters.

`tearDown()` removes all nodes created by the test and removes all temporary local files.

### Running your tests

Running tests is described in the "General" section above, but if you add a new test, you may want to run only it (as opposed to all tests in its file) until you know it works the way you want.

If you want to run a specific test class (for example, `TestMyNewTest` in `islandora_tests.py`), execute a command like:

`python tests/islandora_tests.py TestMyNewTest`

You can also specify multiple test classes within a single test file:

`python tests/islandora_tests.py TestMyNewTest TestMyOtherNewTest`

Of course, one of the main reasons we write tests is to ensure changes in the code don't break existing functionality. Once you are sure your new tests work as intended, please run all tests to detect regression errors.

## Islandora Workbench Integration Drupal module

[Islandora Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) is a Drupal module that allows Islandora Workbench to communicate with Drupal efficiently and reliably. It enables some Views and REST endpoints that Workbench expects, and also provides a few custom REST endpoints (see the module's README for details).

Generally speaking, the only situation where the Integration module will need to be updated (apart from requirements imposed by new versions of Drupal) is if we add a new feature to Workbench that requires a specific View or a specific REST endpoint to be enabled and configured in the target Drupal. If a change is required in the Integration module, it is very important to communicate this to Workbench users, since if the Integration module is not updated to align with the change in Workbench, the new feature won't work.

A defensive coding strategy to ensure that changes in the client-side Workbench code that depend on changes in the server-side Integration module will work is, within the Workbench code, invoke the `check_integration_module_version()` function to check the Integration module's version number and use conditional logic to execute the new Workbench code only if the Integration module's version number meets or exceeds a version number string defined in that section of the Workbench code (e.g., the new Workbench feature requires version 1.1.3 of the Integration module). Under the hood, this function queries `/islandora_workbench_integration/version` on the target Drupal to get the Integration module's version number, although as a developer all you need to do is invoke the `check_integration_module_version()` function and inspect its return value. In this example, your code would compare the Integration module's version number with 1.1.3 (possibly using the `convert_semver_to_number()` utility function) and include logic so that it only executes if the minimum Integration module version is met.

Note that some Views required by Islandora Workbench are defined by users and not by the Islandora Workbench Integration module. Specifically, Views described in the "[Generating CSV files](https://mjordan.github.io/islandora_workbench_docs/generating_csv_files/)" documentation are created by users.
