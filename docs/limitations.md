
!!! note
    If you are encountering problems not described here, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and help improve Islandora Workbench!

## Non-ASCII filenames are normalized to their ASCII equivalents.

The HTTP client library Workbench uses, Requests, requires filenames to be encoded as [Latin-1](https://github.com/psf/requests/issues/4218), while Drupal requires filenames to be encoded as UTF-8. Normalizing filenames that contain diacritics or non-Latin characters to their ASCII equivalents is a compromise. See [this issue](https://github.com/mjordan/islandora_workbench/issues/192) for more information.

If Workbench normalizes a filename, it logs the original and the normalized version.

## Updating nodes does not create revisions.

This is limitation of Drupal (see [this issue](https://github.com/Islandora/documentation/issues/1485)).

## Workbench cannot create taxonomy terms that have required fields other than the core "name" field.

See [this issue](https://github.com/mjordan/islandora_workbench/issues/111) for more information.

## Workbench cannot create a term that has another term as its parent (i.e. terms below the top leve of a hierarchical taxonomy).

See [this issue](https://github.com/mjordan/islandora_workbench/issues/236) for more information.
