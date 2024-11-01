Islandora image media require a value in their "Alternative text" field. This text is used as the `alt` text in the HTML markup rendering the image.

You can assign alt text values to image media by adding the `image_alt_text` field to you CSV file, like this:

```text
file,title,field_model,image_alt_text
IMG_2958.JPG,04,Amsterdam waterfront,25,Amsterdam waterfront on an overcast day.
IMG_5083.JPG,05,Alcatraz Island,25,"Taken from Fisherman's Wharf, San Francisco."
```

The value will only be applied to image media that Workbench creates. If you do not include this field in your CSV file, or the field is present but empty, Workbench will use the node's title as the alt text.

!!! warning
    Workbench only adds alt text to the image media it creates. So if the image files in your create task are "Original file" media use (the default), those are the image media that will get the alt text value from your CSV. If the image files you create are "Service file" media (e.g via an additional_files configuration), those are the ones that get the alt text.

A couple notes:

* Assigning alt text to images is only available in `create` tasks (but see [this issue](https://github.com/mjordan/islandora_workbench/issues/166)).
* Workbench strips out all HTML markup within the alt text to prevent potential cross-site scripting vulnerabilities.
