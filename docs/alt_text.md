Islandora image media require a value in their "Alternative text" field. This text is used as the `alt` text in the HTML markup rendering the image.

You can assign alt text values by adding the `image_alt_text` field to you CSV file, like this:

```text
file,title,field_model,image_alt_text
IMG_2958.JPG,04,Amsterdam waterfront,25,Amsterdam waterfront on an overcast day.
IMG_5083.JPG,05,Alcatraz Island,25,"Taken from Fisherman's Wharf, San Francisco."
```

The value will only be applied to image media. If you do not include this field in your CSV file, Workbench will use the node's title as the alt text. Note that Workbench strips out all HTML markup within the alt text. Also note that this feature is only available in the `create` task.
