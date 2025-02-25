You can delete nodes by providing a CSV file that contains a single column, `node_id`, like this:

```text
node_id
95
96
200
```
Values in the `node_id` column can be numeric node IDs (as illustrated above), full URLs, or full URL aliases.

The config file for update operations looks like this (note the `task` option is 'delete'):

```yaml
task: delete
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: delete.csv
```

Note that when you delete nodes using this method, all media associated with the nodes are also deleted, unless the `delete_media_with_nodes` configuration option is set to `false` (it defaults to `true`). Typical output produced by a `delete` task looks like this:

```text
Node http://localhost:8000/node/89 deleted.
+ Media http://localhost:8000/media/329 deleted.
+ Media http://localhost:8000/media/331 deleted.
+ Media http://localhost:8000/media/335 deleted.
```
Note that taxonomy terms created with new nodes are not removed when you delete the nodes.

!!! note
    Drupal does not allow a user to delete or modify media files unless the user originally created (or is the owner) of the file. This means that if you created a media using "user1" in your Workbench configuration file, only "user1" can delete or modify those files. For `delete` tasks, the value of `username` will need to be the same as the username used to create the original media attached to nodes. If the username defined in a `delete` task is not the same as the Drupal user who owns the files, Drupal will return a `403` response, which you will see in your Workbench logs.

If you want the user to be presented with "You are about to delete [number] nodes and their attached media. Continue? (y/n)" (where `[number]` is the number of node IDs in the input file), add the following to your `delete` task configuration file:

`prompt_user_before_delete_task: true`

If they answer "y", Workbench will proceed; if they answer anything else, Workbench will exit.
