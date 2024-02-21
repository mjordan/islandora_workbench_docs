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
* Integration tests that require a live Islandora instance running at `https://islandora.traefik.me/`
    * `tests/islandora_tests.py`, `tests/islandora_tests_check.py`,  `tests/islandora_tests_hooks.py`, and `tests/islandora_tests_paged_content.py` can be run with `python3 tests/islandora_tests.py`, etc.
    * The Islandora Starter Site deployed with [ISLE](https://github.com/Islandora-Devops/isle-dc) is recommended way to deploy the Islandora used in these tests. Integration tests remove all nodes and media added during the tests, unless a test fails. Taxonomy terms created by tests are not removed.
    * Some integration and field tests output text that beings with "Error:." This is normal, it's the text that Workbench outputs when it finds something wrong (which is probably what the test is testing). Successful test (whether they test for success or failure) runs will exit with "OK". If you can figure out how to suppress this output, please visit [this issue](https://github.com/mjordan/islandora_workbench/issues/160).
* If you want to run the tests within a specific class, include the class name as an argument like this: `python3 tests/unit_tests.py TestCompareStings`


## Adding a new Drupal field type

Eventually, handlers for new Drupal field types will need to be added to Workbench. Currently, the only types supported are:

* strings (for string or text fields) like `Using Islandora Workbench for Fun and Profit`
* integers (for `field_weight`, for example) like `1` or `7281`
* the binary values `1` or `0`
* Existing Drupal-generated entity IDs (term IDs for taxonomy terms or node IDs for collections and parents), which are integers like `10` or `3549`
* structured strings, for typed relation (e.g., `relators:art:30`), link fields (e.g., `https://acme.net%%Acme Products`), geolocation fields (e.g., `"49.16667,-123.93333"`), and authority link data (e.g., `viaf%%http://viaf.org/viaf/10646807%%VIAF Record`)

Details on how to add new field types are coming soon!

## Writing tests

Islandora Workbench's tests are writtin in `unittest`, and, as explained above, fall into two categories:

- Unit tests that do not require a live Islandora instance.
- Integration tests that require a live Islandora instance running at `https://islandora.traefik.me/`.

The unit tests are pretty conventional, but the integration tests are a bit more challenging. The two sample tests provided below are copied from `islandora_tests.py`, and you can see their input files in `tests/assets/create_test` and `tests/assets/max_node_title_length_test`, respectively.

### A simple test

An example of a simple integration test is `TestCreate`, whose code (in `islandora_tests.py`) looks like this:

```python
class TestCreate(unittest.TestCase):

    def setUp(self):
        self.current_dir = os.path.dirname(os.path.abspath(__file__))
        self.create_config_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'create.yml')
        self.create_cmd = ["./workbench", "--config", self.create_config_file_path]

    def test_create(self):
        self.nids = list()
        create_output = subprocess.check_output(self.create_cmd)
        create_output = create_output.decode().strip()
        create_lines = create_output.splitlines()
        for line in create_lines:
            if 'created at' in line:
                nid = line.rsplit('/', 1)[-1]
                nid = nid.strip('.')
                self.nids.append(nid)

        self.assertEqual(len(self.nids), 2)

    def tearDown(self):
        for nid in self.nids:
            quick_delete_cmd = ["./workbench", "--config", self.create_config_file_path, '--quick_delete_node', 'https://islandora.traefik.me/node/' + nid]
            quick_delete_output = subprocess.check_output(quick_delete_cmd)

        self.rollback_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'rollback.csv')
        if os.path.exists(self.rollback_file_path):
            os.remove(self.rollback_file_path)

        self.preprocessed_file_path = os.path.join(self.current_dir, 'assets', 'create_test', 'metadata.csv.preprocessed')
        if os.path.exists(self.preprocessed_file_path):
            os.remove(self.preprocessed_file_path)
```

As you can see, this test runs Workbench using the config file `create.yml`, which lives at `assets/create_test/create.yml`, relative to the workbench directory. A tricky aspect of using real config files in tests is that all paths mentioned in the config file must be relative to the workbench directory. This `create.yml` defines the `input_dir` setting to be `tests/assets/create_test`:

```yaml
task: create
host: https://islandora.traefik.me
username: admin
password: password
input_dir: "tests/assets/create_test"
media_type: image
allow_missing_files: True
```

The test's `setUp()` method prepares the file paths, etc. and within the test's only test method, `test_create()`, runs Workbench using Python's `subprocess.check_output()` method, grabs the node IDs from the output from the "created at" strings emitted by Workbench, adds them to a list, and then counts the number of members in that list. If the number of nodes created matches the expected number, the test passes.

Since this test creates some nodes, we use the test class's `teaarDown()` method to put the target Drupal back into as close a state as we started with as possible. `tearDown()` basically takes the list of node IDs created in `test_create()` and runs Workbench with the `--quick_delete_node` option. It then removes any temporary files created during the test.

### A more complex test

Since Workbench is essentially a specialized REST client, writing integration tests that require interaction with Drupal can get a bit complex. But, the overall pattern is:

1. Create something (nodes, media, taxonomy terms).
1. Confirm that they were created in the expected way (doing this usually involves keeping track of any node IDs needed to run tests or to clean up, and in some cases parsing out values from raw JSON returned by Drupal).
1. Clean up by deleting any Drupal entities created during the tests and also any temporary local files.

An integration test that checks data in the node JSON is `TestUpdateWithMaxNodeTitleLength()`. Here is a copy of its code:

```python
class TestUpdateWithMaxNodeTitleLength(unittest.TestCase):

    def setUp(self):
        self.current_dir = os.path.dirname(os.path.abspath(__file__))
        self.create_config_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'create.yml')
        self.create_cmd = ["./workbench", "--config", self.create_config_file_path]
        self.nids = list()

        self.update_csv_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'update_max_node_title_length.csv')
        self.update_config_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'update.yml')
        self.update_cmd = ["./workbench", "--config", self.update_config_file_path]

        self.temp_dir = tempfile.gettempdir()

    def test_create(self):
        create_output = subprocess.check_output(self.create_cmd)
        self.create_output = create_output.decode().strip()

        create_lines = self.create_output.splitlines()
        for line in create_lines:
            if 'created at' in line:
                nid = line.rsplit('/', 1)[-1]
                nid = nid.strip('.')
                self.nids.append(nid)

        self.assertEqual(len(self.nids), 6)

        # Write out an update CSV file using the node IDs in self.nids.
        update_csv_file_rows = list()
        test_titles = ['This title is 37 chars___________long',
                       'This title is 39 chars_____________long',
                       'This title is 29 _ chars long',
                       'This title is 42 chars________________long',
                       'This title is 44 chars__________________long',
                       'This title is 28 chars long.']
        update_csv_file_rows.append('node_id,title')
        i = 0
        while i <= 5:
            update_csv_file_rows.append(f'{self.nids[i]},{test_titles[i]}')
            i = i + 1
        with open(self.update_csv_file_path, mode='wt') as update_csv_file:
            update_csv_file.write('\n'.join(update_csv_file_rows))

        # Run the update command.
        check_output = subprocess.check_output(self.update_cmd)

        # Fetch each node in self.nids and check to see if its title is <= 30 chars long. All should be.
        for nid_to_update in self.nids:
            node_url = 'https://islandora.traefik.me/node/' + str(self.nids[0]) + '?_format=json'
            node_response = requests.get(node_url)
            node = json.loads(node_response.text)
            updated_title = str(node['title'][0]['value'])
            self.assertLessEqual(len(updated_title), 30, '')

    def tearDown(self):
        for nid in self.nids:
            quick_delete_cmd = ["./workbench", "--config", self.create_config_file_path, '--quick_delete_node', 'https://islandora.traefik.me/node/' + nid]
            quick_delete_output = subprocess.check_output(quick_delete_cmd)

        self.rollback_file_path = os.path.join(self.current_dir, 'assets', 'max_node_title_length_test', 'rollback.csv')
        if os.path.exists(self.rollback_file_path):
            os.remove(self.rollback_file_path)

        self.preprocessed_file_path = os.path.join(self.temp_dir, 'create_max_node_title_length.csv.preprocessed')
        if os.path.exists(self.preprocessed_file_path):
            os.remove(self.preprocessed_file_path)

        # Update test: 1) delete the update CSV file, 2) delete the update .preprocessed file.
        if os.path.exists(self.update_csv_file_path):
            os.remove(self.update_csv_file_path)

        self.preprocessed_update_file_path = os.path.join(self.temp_dir, 'update_max_node_title_length.csv.preprocessed')
        if os.path.exists(self.preprocessed_update_file_path):
            os.remove(self.preprocessed_update_file_path)
```

This test creates some nodes, then writes out a temporary CSV file (which will be used as the `input_csv` file in a subsequent `update` task) containing the new node IDs plus some titles that are longer than `max_node_title_length: 30` setting in the `assets/max_node_title_length_test/update.yml` file. Next, it runs `self.update_cmd` to execute the `update` task. Finally, it fetches the title values for each of the updated nodes and tests the length of each title string to confirm that it does not exceed the maximum allowed length of 30 characters.

