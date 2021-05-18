## Hooks

Islandora Workbench offers three "hooks" that can be used to run scripts at specific points in the Workbench execution lifecycle. The three hooks are:

1. Bootstrap
1. CSV preprocessor
1. Post-action

### Bootstrap scripts

Bootstrap scripts execute prior to Workbench connecting to Drupal. Scripts can be in any language, and need to be executable. For an example of using this feature to run a script that generates sample Islandora content, see the "[Generating sample Islandora content](/islandora_workbench_docs/generating_sample_content/)" section.

To register a bootstrap script in your configuration file, add it to the `bootstrap` option, like this:

```yaml
bootstrap: ["/home/mark/Documents/hacking/workbench/generate_image_files.py"]
```

### CSV preprocessor scripts

CSV preprocessor scripts are applied to CSV values prior to the values being ingested into Drupal. They apply to the entire value from the CSV field and not split field values, e.g., if a field is multivalued, the preprocesor must split it and then reassemble it back into a string before returning it. Note that preprocessor scripts work only on string data and not on binary data like images, etc. and only on custom fields (so not title).

For example, you might want to convert all the values in a CSV field to sentence case. You can do this by writing a small Python script that uses the `capitalize()` method and registering it as a preprocessor. 

To register a preprocessor script in your configuration file, add it to the `preprocessors` option, like this:

```yaml
preprocessors: ["/home/mark/Documents/hacking/workbench/capitalize.py"]
```

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

#### Running multiple scripts in one hook

For all types of hooks, you can register multiple scripts, like this:

```yaml
node_post_create: ["/home/mark/scripts/email_someone.py", "/tmp/hit_remote_api.py"]
```
