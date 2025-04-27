Islandora image media require a value in their "Alternative text" field. This text is used as the `alt` text in the HTML markup rendering the image.

You can assign alt text values to image media using Workbench in two ways, during `create` tasks and using the `add_alt_text` and `update_alt_text` tasks.

###  During `create` tasks

In a `create` task, you can assign alt text values to image media by adding the `image_alt_text` field to you CSV file, like this:

```text
file,title,field_model,image_alt_text
IMG_2958.JPG,04,Amsterdam waterfront,25,Amsterdam waterfront on an overcast day.
IMG_5083.JPG,05,Alcatraz Island,25,"Taken from Fisherman's Wharf, San Francisco."
```

The value will only be applied to image media that Workbench creates. If you do not include this field in your CSV file, or the field is present but empty, Workbench will use the node's title as the alt text.

!!! warning
    During `create` tasks, Workbench only adds alt text to the image media it creates. So if the image files in your create task are "Original file" media use (the default), those are the image media that will get the alt text value from your CSV. If the image files you create are "Service file" media (e.g via an additional_files configuration), those are the ones that get the alt text.

###  Using `add_alt_text` and `update_alt_text` tasks

The `add_alt_text` and `update_alt_text` apply the same alt text value to all image media attached to an existing node, regardless of those images' Media Use tags (e.g., Original file, Service file, or Thumbnail). Their input CSV contains two columns, `node_id` and `image_alt_text`, like this:

```text
node_id,image_alt_text
378,Amsterdam waterfront on an overcast day.
389,"Taken from Fisherman's Wharf, San Francisco."
```

For each node, all of its media of type "Image" will get the corresponding alt text value.

Notes:

* In Islandora, only media that have a type of "Image" support alt text. JPEG2000 and TIFF images are "File" media, so do not have alt text attributes.
* Workbench strips out all HTML markup within the alt text to prevent potential cross-site scripting vulnerabilities.
* In `add_alt_text` and `update_alt_text` tasks, you can use URLs instead of node IDs in the `node_id` CSV column.
* In `update_alt_text` tasks, the same `update_mode` options are available as are in `update` tasks -- "replace" (default), "append", and "delete".