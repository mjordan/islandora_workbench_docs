Islandora Workbench provides several ways to create Drupal nodes. The method you should choose depends on whether the nodes you want to create are simple (no children) or complex (with children such as pages or parts), and how your content files (images, PDFs, video files, etc.) are organized:

| Simple or complex | Type of children | Typical Islandora Model values | Source of input data | Workbench task | Start with this documentation |
| --- | --- | --- | --- | --- | --- |
| simple | no children | "Image", "Digital Document", "Video", "Audio" | CSV file containing one row per node; accompanying content files optional | `create` | "[Preparing your data](/islandora_workbench_docs/preparing_data/)", "[Creating nodes without media](/islandora_workbench_docs/nodes_only/)"  |
| simple | no children | "Image", "Digital Document", "Video", "Audio" | no input CSV file, content files only | `create_from_files` | "[Creating nodes from files](/islandora_workbench_docs/creating_nodes_from_files/)"
| complex | other simple nodes | "Compound" | CSV file containing one row per parent and one row per child, accompanying content files optional | `create` | "[With page/child-level metadata](/islandora_workbench_docs/paged_and_compound/#with-pagechild-level-metadata)" |
| complex | multiple page files | "Paged content" for parents, "Page" for children | CSV input file containing one row per parent but not rows for child pages; page files are arranged in subdirectories | `create` with `paged_content_from_directories: true` | "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)" |

Each method requires specific input data structures (CSV and files as applicable) and specific configuration settings. Refer to the linked documentation for details.
