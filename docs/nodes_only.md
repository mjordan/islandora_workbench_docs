During a `create` task, if you want to create nodes but not any accompanying media, for example if you are testing your metadata values or creating collection nodes, you can include the `nodes_only: true` option in your configuration file:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
nodes_only: true
```

If this is present, Islandora Workbench will only create nodes and will skip all media creation. During `--check`, it will ignore anything in your CSV's `files` field (in fact, your CSV doesn't even need a `file` column). If `nodes_only` is `true`, your configuration file for the `create` task doesn't need a `media_use_tid`, `drupal_filesystem`, or `media_type`/`media_types_override` option.
