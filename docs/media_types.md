!!! note
    Drupal's use of Media types (image, video, document, etc.) is distinct from Islandora's use of "model", which identifies an intellectual entity as an image, video, collection, compound object, newspaper, etc.

By default Workbench defines the following file extention to media type mapping:

| File extensions | Media type |
| --- | --- |
| png, gif, jpg, jpeg | image |
| pdf, doc, docx, ppt, pptx | document |
| tif, tiff, jp2, zip, tar | file |
| mp3, wav, aac | audio |
| mp4 | video |
| txt | extracted_text |

If a file's extension is not defined in either this default mapping, the media is assigned the "file" type.

If you need to override this default mappping, you can do so in two ways:

1. If the override applies to all files named in your CSV's `file` column, use the `media_type` configuration option, for example `media_type: document`). Use this option if all of the files in your batch are to be assigned the same media type, but their extensions are not defined in the default mapping or you wish to override the default mapping.
1. On a per file extension basis, via a mapping in the `media_types_override` option in your configuration file like this one:

```yaml
media_types_override:
  - video: ['mp4', 'ogg']
```
   Use the `media_types_override` option if each of the files named in your CSV's `file` column are to be assigned an extension-specific media type, and their extensions are not defined in the default mapping (or add to the extensions in the default mapping, as in this example).

Note that:

* If a file's extension is not present in the default mapping or in the `media_types_override` custom mapping, the media is assigned the "file" type.
* If you use the `media_types_override` configuration option, your mapping replaces Workbench's default mappings for the specified file extension. This means that if you want to retain any of the default mappings for the file extension, you need to include them in the mapping, as illustrated by the presence of "mp4" in the example above.
* If both `media_type` and `media_types_override` are included in the config file, the mapping in `media_types_override` is ignored and the media type assigned in `media_type` is used.

## Configuring a custom media type

Islandora ships with a set of default media types, including audio, document, extracted text, file, FITS technical metadata, image, and video. If you want to add your own custom media type, you need to tell Workbench two things:

1. which file extension(s) should map to the new media type, and
1. which field on the new media type is used to store the file associated with the media.

To satisfy the first requirement, use the `media_type` or `media_types_override` option as described above. To satisfy the second requirement, use Workbench's `media_file_fields` option. 

The values in the `media_file_fields` option are the machine name of the media type and the machine name of the "File" field configured for that media. To determine the machine name of your media type,

1. go to the field configuration of your media types (Admin > Structure > Media types)
1. choose your custom media type
1. choose the "Manage fields" operation for the media type. The URL of the Drupal page you are now at should look like `/admin/structure/media/manage/my_custom_media/fields`. The machine name of the media is in the second-last position in the URL. In this example, it's `my_custom_media`.
1. in the list of fields, look for the one that says "File" in the "Field type" column
1. the field machine name you want is in that row's "Machine name" column.

Here's an example that tells Workbench that the custom media type "Custom media" uses the "field_media_file" field:

```yaml
media_file_fields:
 - my_custom_media: field_media_file
```

Put together, the two configuration options would look like this:

```yaml
media_types_override:
  - my_custom_media: ['cus']
media_file_fields:
 - my_custom_media: field_media_file
```

In this example, your Workbench job is creating media of varying types (for example, images, videos, and documents, all using the default extension-to-media type mappings. If all the files you are adding in the Workbench job all have the same media type (in the following example, your "my_custom_media" type), you could use this configuration:

```yaml
media_type: my_custom_media
media_file_fields:
 - my_custom_media: field_media_file
```

