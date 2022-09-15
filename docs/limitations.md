
!!! note
    If you are encountering problems not described here, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and help improve Islandora Workbench!

## Non-ASCII filenames are normalized to their ASCII equivalents.

The HTTP client library Workbench uses, Requests, requires filenames to be encoded as [Latin-1](https://github.com/psf/requests/issues/4218), while Drupal requires filenames to be encoded as UTF-8. Normalizing filenames that contain diacritics or non-Latin characters to their ASCII equivalents is a compromise. See [this issue](https://github.com/mjordan/islandora_workbench/issues/192) for more information.

If Workbench normalizes a filename, it logs the original and the normalized version.

## Updating nodes does not create revisions.

This is limitation of Drupal (see [this issue](https://github.com/Islandora/documentation/issues/1485)).

## Password prompt always fails first time, and prompts a second time (which works)

[Issue](https://github.com/mjordan/islandora_workbench/issues/450).

## Workbench can't create/update Paragraphs

There is a bug in Paragraphs' REST interface ([issue](https://github.com/mjordan/islandora_workbench/issues/292)).


## HTML markup in CSV field values is not rendered

Currently, if you include HTML markup in Workbench's CSV input data, that markup is not rendered when users view the resulting node. This is true even if the node's field is correctly configured to accept Drupal's text formats like "basic_html" and "full_html". See issue [#367](https://github.com/mjordan/islandora_workbench/issues/367) for more information.

## Workbench doesn't support taxonomy reference fields that use the "Filter by an entity reference View" reference type

Only taxonomy reference fields that use the "Default" reference type are currently supported. However, [work is being done](https://github.com/mjordan/islandora_workbench/issues/452) to support both types of entity reference.

