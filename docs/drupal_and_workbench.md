This page highlights the most important Drupal and Islandora features relevant to the use of Workbench. Its audience is managers of Islandora repositories who want a primer on how Drupal, Islandora, and Workbench relate to each other. The ideas introduced here are documented in detail elsewhere in this site.

This page is not intended to be a replacement for the [official Islandora documentation](https://islandora.github.io/documentation/), which provides comprehensive and detailed information about how Islandora works.

Your feedback on the usefulness of this page is very important! Join the `#islandoraworkbench` channel in the [Islandora Slack](https://islandora.slack.com/join/shared_invite/zt-1ac9k1xs5-Hbeit2twqexyJCxZQg9ZBQ#/shared-invite/email), or leave a comment on [this Github issue](https://github.com/mjordan/islandora_workbench/issues/532).

## Why would I want to use Islandora Workbench?

Islandora Workbench lets you manage content in an Islandora repository at scale. Islandora provides web forms for creating and editing content on an item-by-item basis, but if you want to load a large number of items into an Islandora repository (or update or delete content in large numbers), you need a batch-oriented tool like Workbench. Islandora Workbench fills a basic need: it enables you to get batches of content into an Islandora repository, and also update or delete content in batches.

## How do I use Islandora Workbench?

Islandora Workbench provides the ability to perform a set of "tasks". The focus of this page is the "create" task, but other tasks Workbench enables include "update", "delete", and "add_media".

To use Islandora Workbench to create new content, you assemble a CSV file containing metadata describing your content, and arrange the accompanying image, video, PDF, and other files so their locations are also included in the CSV file. You then create a configuration file to tell Workbench the URL of your Islandora, the user credentials to use, and the location of your CSV file. You can customize the way Islandora Workbench uses by including various settings in your configuration file.

When you have all these things ready, you use Workbench to "check" your configuration and input data, and, when your checks are complete, you use Workbench to push your content into your Islandora repository.

## Content types, fields, and nodes

Now to shift to the Drupal and Islandora concepts that will help you  use Workbench effectively.

### Content types

!!! info inline end "Relevance to using Workbench"
    Islandora Workbench can only work with a single content type at a time. You define this content type in the `content_type` configuration setting.

Drupal categorizes what people see as "pages" on a Drupal website into content types. By default, Drupal provides "Article" and "Basic Page" content types, but site administrators can create custom content types. You can see the content types configured on your Drupal by logging in as an admin user and visiting `/admin/structure/types`. Or, you can navigate to the list of your site's content types by clicking on the Structure menu item, then the Content Types entry:

![ISLE default content types](images/ISLE_DC_demo_content_types.png)

Islandora, by default, creates a content type called a "Repository Item". But, many Islandora sites use additional content types, such as "Collection".

### Fields

!!! info inline end "Relevance to using Workbench"
    The fields in the content type identified in your Workbench configuration file correspond to columns in your CSV file. Also, you need to be familiar with how to determine a field's "machine name" and "type", as explained below.

The main structural difference between content types in Drupal is each is made up of a unique set of fields. A field in Drupal the same as a "field" in metadata - it is a container for an individual piece of data. For example, all content types have a "title" field (although it might be labeled differently) to hold the page's title. Islandora's default content type, the Repository Item, uses metadata-oriented fields like "Copyright date", "Extent", "Resource type", and "Subject".

Fields have two properties which you need to be familiar with in order to use Islandora Workbench:

1. machine name
1. type

To help explain why these two properties are important, we will use the following screenshot showing the default fields in the "Repository item" content type:

![Default fields in the Repository Item content type](images/repository_item_fields.png)

!!! info inline end "Relevance to using Workbench"
    You must use field machine names in Islandora Workbench configuration files. You can also use them as column headers in your metadata CSV files (but in most cases you can use fields' human-readable labels as column headers instead).

A field has a human-readable label, such as "Copyright date", but that label can change or can be translated, and, more significantly, doesn't need to be unique within a Drupal website. Drupal assigns each field a "machine name" that is easier for software to use than human-readable labels. These field machine names are all lower case, use underscores instead of spaces, and are guaranteed to be unique. In the screenshot above, you can see the machine names in the middle column (you might need to zoom in!). For example, the machine name for the "Copyright date" field is `field_copyright_date`.

A field's "type" determines the structure of the data it can hold. Some common field types used in Islandora are "Text" (and its derivatives "Text (plain)" and "Text (plain, long)"), "Entity Reference", "Typed Relationh", and "EDTF". These field types are explained in the "[Field Data (CSV and Drupal](/islandora_workbench_docs/fields/) documentation, but the important point here is that they are all represented differently in your Workbench CSV. For example:

* EDTF fields take dates in the Library of Congress' [Extended Date/Time Format](https://www.loc.gov/standards/datetime/) (an example CSV entry is `1964/2008`)
* Entity reference fields are used for taxonomy terms (an example entry is `cats:Tabby`, where "cats" is the name of the taxonomy and "Tabby" is the term)
* Typed relation fields are used for taxonomy entries that contain additional data indicating what "type" they are, such as using MARC relators to indicate the relationship of the taxonomy term to the item being described. An example typed relation CSV value is `relators:art:Jordan, Mark`, where "relators:art" indicates the term "Jordan, Mark" uses the MARC relator for "author".


!!! info inline end "Relevance to using Workbench"
    Drupal enforces cardinality very strictly. For this reason, if your CSV file contains more values for a field than the field's configuration allows, Workbench will truncate the number of values to match the maximum number allowed for the field. If it does this, it will leave an entry in its log so you know that it didn't add all the values in your CSV data.

Another important aspect of Drupal fields is their cardinality, or in other words, how many individual values they are configured to have. This is similar to the "repeatability" of fields in metadata schemas. Some fields are configured to hold only a single value, others to hold a a maximum number of values (three, for example), and others can hold an unlimited number of values. You can find each field's cardinality in its "Field settings" tab. Here is an example showing a field with unlimited cardinality:

![Default fields in the Repository Item content type](images/sample_field_cardinality.png)

### Nodes

!!! info inline end "Relevance to using Workbench"
    In Islandora, a node is a metadata description - a set of date, contained in fields, that describe an item. Each row in your input CSV contains the field data that is used to create a node.

Think of a "node" as a specific page in a Drupal website. Every node has a content type (e.g. "Article" or "Repository Item") containing content in the fields defined by its content type. It has a URL in the Drupal website, like `https://mysite.org/node/3798`. The "3798" at the end of the URL is the node ID (also known as the "nid") and uniquely identifies the node within its website.

For some operations you use Islandora Workbench for, such as updating nodes or adding media to nodes, your CSV uses node IDs.

## Taxonomies (a.k.a. vocabularies)

!!! info inline end "Relevance to using Workbench"
    Islandora Workbench lets you create taxonomy term in advance of the nodes they are attached to, or at the same time as the nodes. Also, within your CSV file, you can use term IDs, term URIs, or term names. You can use term names both when you are creating new terms on the fly, or if you are assigning existing terms to newly created nodes.

One of Drupal's most powerful features is its support for structured taxonomies (sometimes referred to as "vocabularies"). These can be used to maintain local authority lists of names, subjects, and other concepts, just like in other library/archives/museum tools. Islandora uses several taxonomies extensively as part of its data model. These include Islandora Models (which determines how derivatives are generated for example) and Islandora Media Use (which indicates if a files is an "Original file" or a "Service file", for example).

Drupal assigns each term an ID, much like it assigns each node an ID. These are called "term IDs" (or "tids"). Like node IDs, they are unique within a single Drupal instance but they are not unique across Drupal instances.

The taxonomies created by Islandora, such as Islandora Models and Islandora Media Use, include Linked Data URIs in the taxonomy term entries. These URIs are useful because they uniquely and reliably identify taxonomy terms across Drupal instances. For example, the taxonomy term with the Linked Data URI `http://pcdm.org/use#OriginalFile` is the same in two Drupal instances even if the term ID for the term is 589 in one instance and 23 in the other. If you create your own taxonomies, you can also assign earch term a Linked Data URI. In your Workbench CSV files, you can use URIs instead of term IDs.

## Media

!!! info inline end "Relevance to using Workbench"
    In most cases, the file you upload using Islandora Workbench will be assigned the "Original file" media use term. Islandora will then automatically generate derivatives, such as thumbnails and extracted text where applicable, from that file and create additional media. However, you can use Workbench to upload custom or pregenerated derivatives if you want.

Media in Islandora are the image, video, audio, PDF, and other content files that are attached to nodes. Together, a node and its attached media make up an resource or item.

Media have types. Standard media types defined by Islandora are:

* Audio
* Document
* Extracted text
* FITS Technical Metadata
* File
* Image
* Remote video
* Video

In general when using Workbench you don't need to worry about assigning a type to a file. Workbench infers a media's type from the file extensions, but you can override this if necessary.

Media are also assigned terms from the Islandora Media Use vocabulary. These terms, combined with the media type, determine how the files are displayed to end users and how and what types of derivatives Islandora generates. They can also be useful in exporting content from Islandora and in digital preservation workflows (for example). A selectin of terms from this vocabulary is:

* Original file
* Intermediate file
* Preservation Master File
* Service file
* Thumbnail image
* Transcript
* Extracted text


## Views

!!! info inline end "Relevance to using Workbench"
    You generally don't need to interact with Views when using Islandora Workbench, but you can use Workbench to [export CSV data](/islandora_workbench_docs/generating_csv_files/#using-a-drupal-view-to-identify-content-to-export-as-csv) from Drupal via a View.

Views are another extremely powerful Drupal feature that Islandora uses extensively. A View is a configuration that generates a list of things managed by Drupal, most frequently nodes but also media and taxonomy terms. Islandora Workbench uses an accompanying Drupal module called Islandora Workbench Integration that creates a number of custom Views that are necessary for Workbench to interact with Drupal. So even though as a Workbench user you might not use Views directly, Workbench is getting information from Drupal constantly using a set of custom Views.


## REST

!!! info inline end "Relevance to using Workbench"
    Your log file might contain references to "HTTP response codes" or contain some very ugly JSON (which is the structure used by Drupal and Workbench to communicate with each other). All you need to know is where your Workbench log is, so you can share it for troubleshooting.

As a user of Workbench, you don't ned to know anything about REST. The only exception is that sometimes, if things go wrong, Workbench will include in its log file some details about the particular REST request that didn't work. Again, you don't need to know how Workbench interacts with Drupal via REST. But, if Workbench fails to add your content to Islandora, and you [reach out for help](/islandora_workbench_docs/troubleshooting/) figuring out why, you might be asked to provide your Workbench log file.