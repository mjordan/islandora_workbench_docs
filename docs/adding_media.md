### Adding media from files

You can add media to existing nodes by providing a CSV file with a `node_id` column plus a `file` field that contains the name of the file you want to add:

```text
node_id,file
100,test.txt
```
Values in the `node_id` column can be numeric node IDs (as illustrated above), full URLs, or full URL aliases.

The config file for "add_media" tasks like this (note the `task` option is 'add_media'):

```yaml
task: add_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: add_media.csv
# media_use_tid is optional, it defaults to "Original file".
media_use_tid: 21
```

This is the same configuration file using a term URI in `media_use_tid` rather than a term ID:

```yaml
task: add_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: add_media.csv
media_use_tid: "http://pcdm.org/use#Transcript"
```

If you want to specify a media_use_tid per CSV row, you can include that column in your CSV (in either "add_media" or "create" tasks):

```text
node_id,file,media_use_tid
100,test.txt,21
110,test2.txt,35
```

If you include `media_use_tid` values in your CSV file, they override the `media_use_tid` value set in your configuration file.

!!! note
    If you create an "Extracted Text" media, the contents of the specified text file will be added to the media's 'field_edited_text' field, allowing it to be indexed in Solr.

!!! note
    The Drupal filesystem where files are stored is determined by each media type's file field configuration. It is not possible to override that configuration.

### Adding references to media for use with DGI's Image Discovery module

DiscoveryGarden's [DGI Image Discovery module](https://github.com/discoverygarden/dgi_image_discovery) provides a way to assign the same thumbnail image to multiple nodes. This is not the module's main purpose, but reusing a thumbnail image on many nodes is easy to accomplish using this module and Islandora Workbench.

Essentially, the Image Discovery module defines a specific Drupal field on a node, `field_representative_image`, that contains a reference to an existing media, for example a media with an Islandora Media Use term "Thumbnail image". This approach to defining a thumbnail is different than Islandora's normal node/media relationship, where the media entity references its parent node in a `field_media_of` field attached to the media.

To use Workbench to populate `field_representative_image`, simply include that field in your Workbench `create` task CSV and populate it with the media ID of the thumbnail media you want to use. The following example CSV populates the field with the media ID "3784":

```
id,file,title,field_representative_image,field_model
test-001,Test node 001,3784,Image
test-002,Test node 002,3784,Digital Document
```

Note that this approach to assigning a thumbnail image to a node does not use Workbench's `additional_fields` configuration setting to define a CSV column containing the filename of a thumbnail image. It simply populates a node's `field_representative_image` field with a media ID. DGI's module ensures that the referenced media is used as the node's thumbnail image. No other Workbench configuration is necessary.

It's just as easy to use an `update` task to add the referenced media ID to nodes:

```
node_id,field_representative_image
1089,3784
1093,3784
```
