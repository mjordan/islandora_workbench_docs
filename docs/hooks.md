## Hooks

Islandora Workbench offers three "hooks" that can be used to run scripts at specific points in the Workbench execution lifecycle. The three hooks are:

1. Bootstrap
1. CSV preprocessor
1. Post-action

Hook scripts can be in any language, and need to be executable by the user running Workbench.

Execution (whether successful or not) of hook scripts is logged, including the scripts' exit code.

### Bootstrap and shutdown scripts

Bootstrap scripts execute prior to Workbench connecting to Drupal. For an example of using this feature to run a script that generates sample Islandora content, see the "[Generating sample Islandora content](/islandora_workbench_docs/generating_sample_content/)" section.

To register a bootstrap script in your configuration file, add it to the `bootstrap` option, like this:

```yaml
bootstrap: ["/home/mark/Documents/hacking/workbench/generate_image_files.py"]
```

Each bootstrap script gets passed a single argument, the path to the Workbench config file that was specified in Workbench's `--config` argument. For example, if you are running Workbench with a config file called `create.yml`, `create.yml` will be passed as the argumement to your bootstrap script. 

Shutdown scripts work the same way as bootstrap scripts but they execute after Workbench has finished connecting to Drupal. A common situation where a shutdown script is useful is to check the Workbench log for failures, and if any are detected, to email someone. To register a shutdown script, add it to the `shutdown` option:

```yaml
shutdown: ["/home/mark/Documents/hacking/workbench/shutdown_example.py"]
```

`--check` will check for the existence of bootstrap and shutdown scripts, and that they are executable, but does not execute them. The scripts are only executed when Workbench is run without `--check`.

Very basic example bootstrap and shutdown scripts can be found in the `scripts` folder.

### CSV preprocessor scripts

CSV preprocessor scripts are applied to CSV values prior to the values being ingested into Drupal. They apply to the entire value from the CSV field and not split field values, e.g., if a field is multivalued, the preprocesor must split it and then reassemble it back into a string. Note that preprocessor scripts work only on string data and not on binary data like images, etc. and only on custom fields (so not title). If you are interested in seeing preprocessor scripts act on binary data such as images, see this [issue](https://github.com/mjordan/islandora_workbench/issues/45).

For example, you might want to convert all the values in a CSV field to sentence case. You can do this by writing a small Python script that uses the `capitalize()` method and registering it as a preprocessor. 

To register a preprocessor script in your configuration file, add it to the `preprocessors` option, like this:

```yaml
preprocessors: ["/home/mark/Documents/hacking/workbench/scripts/samplepreprocessor.py"]
```

Each preprocessor script gets passed two arguments:

1. the character used as the CSV subdelimiter (defined in the `subdelimiter` config option, which defaults to `|`)
1. the CSV field value

### Post-action scripts

Post-action scripts execute after a node is created or updated, or after a media is created.  

To register post-action scripts in your configuration file, add them to either the `node_post_create`, `node_post_update`, or `media_post_create` configuration setting:

```yaml
node_post_create: ["/home/mark/Documents/hacking/workbench/post_node_create.py"]
```

```yaml
node_post_update: ["/home/mark/Documents/hacking/workbench/post_node_update.py"]
```

```yaml
media_post_create: ["/home/mark/Documents/hacking/workbench/post_media_update.py"]
```

The arguments passed to each post-action hook are:

1. the path to the Workbench config file that was specified in the `--config` argument
1. the HTTP response code returned from the action (create, update), e.g. `201` or `403`. Note that this response code is a string, not an integer.
1. the entire HTTP response body; this will be raw JSON.

These arguments are passed to post-action scripts automatically. You don't specific them when you register your scripts in your config file. The `scripts/entity_post_task_example.py` illustrates these arguments.

Your scripts can find the entity ID and other information within the (raw JSON) HTTP response body. Using the way Python decodes JSON as an example, if the entity is a node, its nid is in `entity_json['nid'][0]['value']`; if the entity is a media, the mid is in `entity_json['mid'][0]['value']`. The exact location of the nid and mid may differ if your script is written in a language that decodes JSON differently than Python (used in this example) does.

#### Running multiple scripts in one hook

For all types of hooks, you can register multiple scripts, like this:

```yaml
bootstrap: ["/home/mark/Documents/hacking/workbench/bootstrap_example_1.py", "/home/mark/Documents/hacking/workbench/bootstrap_example_2.py"]
shutdown: ["/home/mark/Documents/hacking/workbench/shutdown_example_1.py", "/home/mark/Documents/hacking/workbench/shutdown_example_2.py"]
node_post_create: ["/home/mark/scripts/email_someone.py", "/tmp/hit_remote_api.py"]
```

They are executed in the order in which they are listed.
