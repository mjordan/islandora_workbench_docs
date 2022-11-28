You can delete individual nodes and media by using the `--quick_delete_node` and `--quick_delete_media` command-line options, respectively. In both cases, you need to provide the full URL (or URL alias) of the node or media you are deleting. For example:

* `./workbench --config anyconfig.yml --quick_delete_node http://localhost:8000/node/393`
* `./workbench --config anyconfig.yml --quick_delete_media http://localhost:8000/media/552/edit`

In the case of the `--quick_delete_node` option, all associated media and files are also deleted (but see the exception below); in the case of the `--quick_delete_media` option, all associated files are deleted.

You can use any valid Workbench configuration file regardless of the `task` setting, since the only settings the quick delete tool requires are `username` and `password`. All other configuration settings are ignored with the exception of:

* `delete_media_with_nodes`. If you want to not delete a node's media when using the `--quick_delete_node` option, your configuration file should include `delete_media_with_nodes: false`.
* `standalone_media_url`. If your Drupal instance has the "Standalone media URL" option at `/admin/config/media/media-settings` unchecked (the default), you will need to include `/edit` at the end of media URLs. Workbench will tell you if you include or exclude the `/edit` incorrectly. If your Drupal has this option checked, you will need to include `standalone_media_url: true` in your configuration file. In this case, you should not include the `/edit` at the end of the media URL.
