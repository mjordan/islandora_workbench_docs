
!!! note
    If you are encountering problems not described here, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and help improve Islandora Workbench!

## `parent_id` CSV column can only contain one ID

The `parent_id` column can contain only a single value. In other words, values like `id_0029|id_0030` won't work. If you want an item to have multiple parents, you need to use a later `update` task to assign additional values to the child node's `field_member_of` field.

## Non-ASCII filenames are normalized to their ASCII equivalents.

The HTTP client library Workbench uses, Requests, requires filenames to be encoded as [Latin-1](https://github.com/psf/requests/issues/4218), while Drupal requires filenames to be encoded as UTF-8. Normalizing filenames that contain diacritics or non-Latin characters to their ASCII equivalents is a compromise. See [this issue](https://github.com/mjordan/islandora_workbench/issues/192) for more information.

If Workbench normalizes a filename, it logs the original and the normalized version.

## Updating nodes does not create revisions.

This is limitation of Drupal (see [this issue](https://github.com/Islandora/documentation/issues/1485)).

## Password prompt always fails first time, and prompts a second time (which works)

More information is available at this [issue](https://github.com/mjordan/islandora_workbench/issues/450).

## Workbench doesn't support taxonomy reference fields that use the "Filter by an entity reference View" reference type

Only taxonomy reference fields that use the "Default" reference type are currently supported. As a work around, to populate a "Filter by an entity reference View" field, you can do the following:

* use term IDs instead of term names or URIs in your input CSV *and*
* include `require_entity_reference_views: false` in your configuration file.

Note that Workbench will not validate values in fields that are configured to use this type of reference. Also, term IDs that are not in the View results will result in the node not being created (Drupal will return a 422 response).

## Workbench doesn't support multivalued Paragraph fields.

Currently, Workbench assumes that all fields defined within [Paragraphs](/islandora_workbench_docs/fields/#paragraphs-entity-reference-revisions-fields) are single-valued. Please see this [Github issue](https://github.com/mjordan/islandora_workbench/issues/887) for updates.

## Setting destination filesystem for media is not possible

Drupal's REST interface for file fields does not allow overriding the "upload destination" (filesystem) that is defined in a media type's file field configuration. For example, if a file field is configured to use the "Flysystem: fedora" upload destination, you cannot tell Workbench to use the "Public Files" upload destination instead.

Note that the `drupal_filesystem` configuration setting only applied to Drupal versions 8.x - 9.1. In Drupal 9.2 or later, this setting is ignored.

## Workbench cannot modify media automatically generated by Islandora's microservices

Islandora uses Contexts to initiate the generation of derivative media. Configuration for these Contexts is available in the "Derivatives" section of `admin/structure/context`. One example of a derivative media is a thumbnail generated on the ingestion of a JPEG original file.

During `create` or `add_media` tasks, Workbench cannot modify or even be aware of derivative media automatically generated by Islandora. For example, Workbench can't add alt text to a thumbnail image automatically created by the "Image derivatives" Context. This is because the derivative media are generated asynchronously by Islandora's job processing queue. In other words, there is no predictable relationship between when an "Original file" media is created by Workbench (or uploaded by a user in the Drupal content management forms) and when the "Thumbnail" media is generated by Islandora's microservices. This unpredictability prevents Workbench from knowing when the derivative will be or has been created, making it impossible to have Workbench automatically add alt text to that thumbnail.

