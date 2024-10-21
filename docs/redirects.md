Islandora Workbench enables you to create redirects managed by the [Redirect](https://www.drupal.org/project/redirect) contrib module. One of the most common uses for redirects is to retain old URLs that have been replaced by newer ones. Within the Islandora context, being able to redirect from Islandora 7.x `islandora/object/[PID]` style URLs to their new Islandora 2 replacements such as `node/[NODEID]`. Using redirects, you can ensure that the old URLs continue to lead the user to the expected content.

In order to use Workbench for this, you must install the Redirect module and configure the "Redirect" REST endpoing at `/admin/config/services/rest` so that it has the following settings:

![Redirect REST endpoint settings](images/redirect_rest_configs.png)

A sample configuration file for a `creatre_redirects` task looks like this:

```
task: create_redirects
host: https://islandora.traefik.me
username: admin
password: password
input_csv: my_redirects.csv
# This setting is explained below.
# redirect_status_code: 302
```

The input CSV contains only two columns, `redirect_source` and `redirect_target`. Each value in the `redirect_source` column is a relative (to the hostname running Drupal) URL path that, when visited, will automatically redirect the user to the relative (to the hostname running Drupal) URL path (or external absolute URL) named in the `redirect_target` column. Here is a brief example CSV:

```
redirect_source,redirect_target
contact_us,how_can_we_help
islandora/object/alpine:482,node/1089
islandora/object/awesomeimage:collection,https://galleries.sfu.ca
node/16729,node/3536
```

Assuming that the hostname of the Drupal instance running the Redirects module is `https://mydrupal.org`, when these redirects are created, the following will happen:

- When a user visits `https://mydrupal.org/contact_us`, they will automatically be redirected to `https://mydrupal.org/how_can_we_help`.
- When a user visits `https://mydrupal.org/islandora/object/alpine:482`, they will be automatically redirected to `https://mydrupal.org/node/1089`.
- When a user visits `https://mydrupal.org/islandora/object/awesomeimage:collection`, they will be automatically redirected to the external (to `https://mydrupal.org`) URL `https://galleries.sfu.ca`.
- When a user visits `https://mydrupal.org/node/16729`, they will be automatically redirected to `https://mydrupal.org/node/3536`.

A few things to note about creating redirects using Islandora Workbench:

- The values in the `redirect_source` column are always relative to the root of the Drupal hostname URL. Drupal expects them to *not* begin with a leading `/`, but if you include it, Workbench will trim it off automatically.
- The `redirect_source` values do not need to represent existing nodes. For example, a value like `islandora/object/awesomeimage:collection` has no underlying node; it's just a path that Drupal listens at, and when requested, redirects the user to the corresponding target URL. Values in `redirect_source` that do have underlying nodes will redirect users to the corresponding `redirect_target` but don't make a lot of sense, since the user will always be automatically redirected and never get to see the underlying node at the source URL. However, you may want to use a `redirect_target` value that has a local node if you don't want users to see that node temporarily for some reason (after that reason is no longer valid, you would remove the redirect).
- Currently, Islandora Workbench can only create redirects, it can't update or delete them. If you have a need for either of those funtionalities, open a Github issue.
- HTTP redirects work by issuing a special response code to the browser. In most cases, and by default in Workbench, this is `301`. However, you can change this to another redirect HTTP status code by including the `redirect_status_code` setting in your config file specifying the code you want Drupal to send to the user's browser, e.g., `redirect_status_code: 302`.


