## Overriding Workbench's default extension to media type mappings

!!! note
    Drupal's use of Media types (image, video, document, etc.) is distinct from Islandora's use of "model", which identifies an intellectual entity as an image, video, collection, compound object, newspaper, etc.

By default Workbench uses the following mapping of file extensions to Islandora media types. Note that this default mapping is intended to cover commonly used file extensions, and does not comprehensively mirror the "Allowed file extensions" settings in all of Islandora media types' file fields:

| File extensions | Media type |
| --- | --- |
| png, gif, jpg, jpeg | image |
| pdf, doc, docx, ppt, pptx | document |
| tif, tiff, jp2, zip, tar | file |
| mp3, wav, aac | audio |
| mp4 | video |
| txt | extracted_text |

If a file's extension is not defined in this default mapping, the media is assigned the "file" type.

If you need to override this default mapping, you can do so in two ways:

1. If the override applies to all files named in your CSV's `file` column, use the `media_type` configuration setting, for example `media_type: document`). Use this setting if all of the files named in your `file` column are to be assigned the same media type, but their extensions are not defined in the default mapping or you wish to override the default mapping.
1. On a per file extension basis, via a mapping in the `media_types_override` option in your configuration file like this one:

```yaml
media_types_override:
  - video: ['mp4', 'ogg']
```

Use the `media_types_override` option if each of the files named in your CSV's `file` column (or in columns defined using the `additional_files` setting) are to be assigned an extension-specific media type, and their extensions are not defined in the default mapping (or add to the extensions in the default mapping, as in this example).

Note that:

* This mapping simply tells Workbench which media type to use for a given file extension. Neither the default mapping nor an overridden mapping needs to mirror the "Allowed file extensions" setting in the target Islandora media type's file field. However, since the "Allowed file extensions" setting restricts what extensions are allowed for a given media type, that Drupal configuration setting ultimately restricts what extensions are allowed for the target media type.
* Mappings defined in the `media_types_override` config setting apply to all files Workbench is ingesting, regardless of whether they are in the `file` CSV column or in a column defined in the `additional_files` setting.
* If a file's extension is not present in the default mapping or in the `media_types_override` custom mapping, the media is assigned the "file" type.
* If you use the `media_types_override` configuration option, your mapping *replaces* Workbench's default mappings for the specified media type. This means that if you want to retain the default media type mapping for a file extension, you need to include it in the mapping, as illustrated by the presence of "mp4" in the example above.
* If both `media_type` and `media_types_override` are included in the config file, the mapping in `media_types_override` is ignored and the media type assigned in `media_type` is used.

## Overriding Workbench's default MIME type to file extension mappings

For remote files, in other words files that start with `http` or `https`, Workbench relies on the MIME type provided by the remote web server to determine the extension of the temporary file that it writes locally. If you are getting errors indicating that a file extension is not registered for a given media type, and you suspect that the extensions are wrong, you can include the `mimetype_extensions` setting in your config file to tell Workbench which extensions to use for a given MIME type. Here is a (hypothetical) example that tells Workbench to assign the '.foo' extension to files with the MIME type 'image/jp2' and the extension '.bar' to files with the MIME type 'image/jpeg':

```yaml
mimetype_extensions:
  'image/jp2': '.foo'
  'image/jpeg': '.bar'
```

## Overriding Workbench's default file extension to MIME type mappings

It may be necessary to assign a media a MIME type that is different from the MIME type that Drupal ordinarily derives from a given extension. The best example of this is that media for hOCR files need to have the MIME type `text/vnd.hocr+html`. Without explicitly indicating that MIME type, Drupal will assign the media for an .hocr file, on creation, the catch-all MIME type `application/octet-stream`.

Workbench automatically assigns files ending in `.hocr` the correct MIME type. But, if you want to override that for some reason, or want to tell Workbench to create a media with a specific MIME type from a file with a specific extension, you can add to your configuration file a an extension-to-MIME-type mapping like this (the leading `.` in the extension on the left is optional):

```yaml
extensions_to_mimetypes:
  'mbox': 'application/mbox'
```

## Assigning a media type by Media Use URI

We typically assign a media type to a file based on the file's extension. However, there are instances where multiple media types allow uploading of files with the same file extension. One example is FITS XML: while files with the "xml" extension can be assigned a `file` media type, the `fits_technical_metadata` media type is more suitable in this case.

To assign a media type to a file based on the file's media use term as defined in the `additional_files` config setting, use `media_type_by_media_use`. Below is an example demonstrating how to assign a FITS XML file to the `fits_technical_metadata` media type (the media use term's URI is on the left side of the mapping, with the target media type's machine name on the right):

```yaml
media_type_by_media_use:
  - https://projects.iq.harvard.edu/fits: fits_technical_metadata
```

Since a set of files' media use term is configured in `additional_files`, you would only ever use `media_type_by_media_use` in conjunction with `additional_files`.

## Configuring a custom media type

Islandora ships with a set of default media types, including audio, document, extracted text, file, FITS technical metadata, image, and video. If you want to add your own custom media type, you need to tell Workbench two things:

1. which file extension(s) should map to the new media type, and
1. which field on the new media type is used to store the file associated with the media.

To satisfy the first requirement, use the `media_type` or `media_types_override` option as described above. To satisfy the second requirement, use Workbench's `media_type_file_fields` option.

The values in the `media_type_file_fields` option are the machine name of the media type and the machine name of the "File" field configured for that media. To determine the machine name of your media type,

1. go to the field configuration of your media types (Admin > Structure > Media types)
1. choose your custom media type
1. choose the "Manage fields" operation for the media type. The URL of the Drupal page you are now at should look like `/admin/structure/media/manage/my_custom_media/fields`. The machine name of the media is in the second-last position in the URL. In this example, it's `my_custom_media`.
1. in the list of fields, look for the one that says "File" in the "Field type" column
1. the field machine name you want is in that row's "Machine name" column.

Here's an example that tells Workbench that the custom media type "Custom media" uses the "field_media_file" field:

```yaml
media_type_file_fields:
 - my_custom_media: field_media_file
```

Put together, the two configuration options would look like this:

```yaml
media_types_override:
  - my_custom_media: ['cus']
media_type_file_fields:
 - my_custom_media: field_media_file
```

In this example, your Workbench job is creating media of varying types (for example, images, videos, and documents, all using the default extension-to-media type mappings. If all the files you are adding in the Workbench job all have the same media type (in the following example, your "my_custom_media" type), you could use this configuration:

```yaml
media_type: my_custom_media
media_type_file_fields:
 - my_custom_media: field_media_file
```

