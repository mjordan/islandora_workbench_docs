Islandora Workbench lets you load vocabularies from CSV files. This ability is separate from creating vocabulary terms at the same time as creating the nodes in `create` or `update` tasks, as described in "[Drupal fields and CSV fields](/islandora_workbench_docs/fields)" documentation. You should load a vocabulary using the options described here if any of these situations applies to you:

* you are working with a vocabulary that has fields in additiont term name.
* you are working with a vocabulary that is hierarchical.
* you want to use Workbench to load vocabulary terms but you want them to exist before (or separate from) the nodes that a `create` task creates.

If you want to create terms at the same time you are using a `create` task to create the nodes the terms are attached to, and if the terms you are creating don't have any additional fields or hierarchical relationships to other terms, you can still create terms as described in as described in the "Taxonomy reference fields" section of "[Drupal fields and CSV fields](/islandora_workbench_docs/fields)."

### Vocabularies with fields

To load a vocabulary, you use a `create_terms` task. A typical `create_terms` configuration file looks like this:

```yaml
task: create_terms
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: my_term_data.csv
vocab_id: myvocabulary
```

The `vocab_id` config option is required. It contains the machine name of the vocabulary you are loading the terms into. 

The CSV file identified in the `input_csv` option has one required column, `term_name`, which contains each term's name. (Another reserved but optional column, `parent`, is described below in the "Hierarchical vocabularies" section.) You can add additional columns that correspond to a vocabulary's field names, just like you do when you assemble your CSV for `create` tasks.

!!! note
    Unlike Input CSV files used during `create` tasks, input CSV files for `create_terms` tasks do not have an "id" column. Instead, `term_name` is the column whose values are the unique identifier for each term. Workbench assumes that term names are unique within a vocabulary. If the terms in the `term_name` column aren't unique, Workbench only creates the term the first time it encounters it in the CSV file. 

An example CSV for a vocabulary that has two additional fields, "field_example" and "field_second_example", my input CSV's column headers would look like this:

```text
term_name,field_example,field_second_example

```

Optional fields don't need to be included in our CSV if you are not populating them, but required fields do need to be present, and populated. Running `--check` on a `create_terms` task will flag any required fields that are missing from your input CSV file.


### Hierarchical vocabularies

If you want to create a vocabulary that is hierarchical, like this:

![Hierarchical_vocabulary](images/hierarchical_vocab.png)

you can add a `parent` column to your CSV and in it, include the term name of the term you want as the parent. For example, the above sample vocabulary was created using this CSV input file:

```text
term_name,parent
Automobiles,
Sports cars,Automobiles
SUVs,Automobiles
Jaguar,Sports cars
Porche,Sports cars
Land Rover,SUVs
```

One important aspect of creating a hierarchical vocabulary is that all parents must exist before their children. That means that within your CSV file, the rows for terms used as parents should be placed earlier than the rows for their children. If a term is named as a parent but doesn't exist (or doesn't exist yet because it came after the child term in the CSV), Workbench will create the child term and write a warning in the log indicating that the parent didn't exist at the time of creating the child. 

You can include the `parent` column in your CSV along with Drupal field names. Workbench will not only create the hierarchy, it will also add the field data to the terms:

```text
term_name,parent,field_external_uri
Automobiles,,
Sports cars,Automobiles,https://en.wikipedia.org/wiki/Sports_car
SUVs,Automobiles,https://en.wikipedia.org/wiki/Sport_utility_vehicle
Jaguar,Sports cars,
Porche,Sports cars,
Land Rover,SUVs,
```
