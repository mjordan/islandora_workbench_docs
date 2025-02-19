Workbench can put substantial stress on Drupal. In some cases, this stress can lead to instability and errors.

!!! note
    The options described below modify Workbench's interaction with Drupal. They do not have a direct impact on the load experienced by the microservices Islandora uses to do things like indexing node metadata in its triplestore, extracting full text for indexing, and generating thumbnails. However, since Drupal is the "controller" for these microservices, reducing the number of Workbench requests to Drupal will also indirectly reduce the load experienced by Islandora's microservices.


### Pausing

The main way to use Workbench's configuration settings to reduce load on Drupal is by telling it to pause at specific times. There are two types of pausing, 1) basic and 2) adaptive. Both types of pausing improve stability and reliability by slowing down Workbench's overall execution time.

#### Basic pause

The `pause` configuration setting tells Workbench to temporarily halt execution before every request, thereby spreading load caused by the requests over a longer period of time. To enable `pause`, include the setting in your configuration file, indicating the number of seconds to wait between requests:

```yaml
pause: 2
```

Using `pause` will help decrease load-induced errors, but it is inefficient because it causes Workbench to pause between *all* requests, even ones that are not putting stress on Drupal. A useful strategy for refining Workbench's load-reduction capabilities is to try `pause` first, and if it reduces errors, then disable `pause` and try `adaptive_pause` instead. `pause` will confirm that Workbench is adding load to Drupal, but `adaptive_pause` will tell Workbench to pause only when it detects in real-time that specific requests are putting load on Drupal.

!!! note
    `pause` and `adaptive_pause` are mutually exclusive. If you include one in your configuration files, you should not include the other.

#### Adaptive pause

Adaptive pause only halts execution between requests if Workbench detects that Drupal is slowing down. It does this by comparing Drupal's response time for the most recent request to the average response time of the 20 previous requests made by Islandora Workbench. If the response time for the most recent request reaches a specific threshold, Workbench's adaptive pause will kick in and temporarily halt execution to allow Drupal to catch up. The number of previous requests used to determine the average response time, 20, cannot be changed with a configuration setting.

The threshold that needs to be met is configured using the `adaptive_pause_threshold` setting. This setting's default value is 2, which means that the adaptive pause will kick in if the response time for the most recent request Workbench makes to Drupal is 2 times (double) the average of Workbench's last 20 requests. The amount of time that Workbench will pause is determined by the value of `adaptive_pause`, which, like the value for `pause`, is a number of seconds (e.g., `adaptive_pause: 3`). You enable adaptive pausing by adding the `adaptive_pause` setting to your configuration file.

Here are a couple of examples. Keep in mind that `adaptive_pause_threshold` has a default value (2), but `adaptive_pause` does not have a default value. The first example enables `adaptive_pause` using the default value for `adaptive_pause_threshold`, telling it to pause for 3 seconds between requests if Drupal's response time to the last request is 2 times slower (`adaptive_pause_threshold`'s default value) than the average of the last 20 requests:

```yaml
adaptive_pause: 3
```

In the next example, we override `adaptive_pause_threshold`'s default by including the setting in the configuration:

```yaml
adaptive_pause: 2
adaptive_pause_threshold: 2.5
```

In this example, adaptive pausing kicks in only if the response time for the most recent request is 2.5 times the average of the response time for the last 20 requests. You can increment `adaptive_pause_threshold`'s value by .5 (e.g., 2.5, 3, 3.5, etc.) until you find a sweet spot that balances reliability with overall execution time. You can also decrease or increase the value of `adaptive_pause` incrementally by intervals of .5 to further refine the balance - increasing `adaptive_pause`'s value lessens Workbench's impact on Drupal at the expense of speed, and decreasing its value increases speed but also impact on Drupal.

Since `adaptive_pause` doesn't have a default value, you need to define its value in your configuration file. Because of this, using `adaptive_pause_threshold` on its own in a configuration, e.g.:

```yaml
adaptive_pause_threshold: 3
```

doesn't do anything because `adaptive_pause` has no value. In other words, you can use `adaptive_pause` on its own, or you can use `adaptive_pause` and `adaptive_pause_threshold` together, but it's pointless to use `adaptive_pause_threshold` on its own.

#### Logging Drupal's response time

If a request is paused by adaptive pausing, Workbench will automatically log the response time for the next request, indicating that `adaptive_pause` has temporarily halted execution.

You may want to log Drupal's response time regardless of whether `adaptive_pause` had kicked in or not to aid in troubleshooting. To do this, add `log_response_time: true` to your configuration file.

All logging of response time, whether logged during adaptive pause or because you have specified response time logging in your config file, includes variation from the average of the last 20 response times.

### Ingesting pregenerated derivatives

The strategy for reducing load on Drupal described in this section is not implemented directly via Workbench configuration, but applies to your content-preparartion workflows.

Islandora's system for generating [derivatives](https://islandora.github.io/documentation/concepts/derivatives/) uses Drupal Actions to communicate with microservices such as Crayfits, Homarus and Hypercube. Even though these microservices typically run in their own Docker containers, they can get overwhelmed and result in failed derivative creation. Sometimes the load on these microservices manifests itself as load on the Drupal server, sometimes it doesn't. At a minimum, the Action that initiates Drupal's communication with the microservices will add additional load.

A strategy for reducing the use of derivative-generating microservices is to ingest the derivatives (OCR/hOCR, thumbnails, FITS XML, etc.) along with the original file. Workbench can do this in two main ways, 1) through the use of the `additional_files` configuration setting (docs [here](/islandora_workbench_docs/adding_multiple_media/)) and, when 2) ingesting paged content, ingesting pregenerated OCR and hOCR (docs [here](/islandora_workbench_docs/paged_and_compound/#ingesting-ocr-and-other-files-with-page-images)).

!!! warning
    If you are ingesting pregenerated derivatives, you should temporarily disable Contexts that would normally generate derivatives with the same Islandora Media Use terms as the derivatives you are ingesting. Do this at `admin/structure/context` by choosing "Disable" in the "Operations" list for each applicable Context, and be sure to re-enable them after running Workbench. Because Islandora creates derivatives asynchronously, it is impossible to guarantee that Contexts will not overwrite additional Media created using this feature. Running `--check` will warn you of this by issuing a message like the following: "Warning: Term ID '18' registered in the 'additional_files' config option for field 'thumbnail' will assign an Islandora Media Use term that might conflict with derivative media. You should temporarily disable the Context or Action that generates those derivatives."

### Changing the default behavior of how Workbench interacts with Drupal

Islandora Workbench is basically a batch-oriented, long-running REST client for Drupal. In order to improve performance and stability, it implements HTTP request caching and HTTP request retries. While both of these mechanisms are configurable, changing their defaults is not the preferred way to reduce Workbench's load on Drupal.

Before changing their defaults, you should exhaust the possibilities of pausing and ingesting pregenerated derivatives as described above.

#### Caching

Caching outbound HTTP requests minimizes Workbench's overall interaction with Drupal, reducing the load on Drupal and speeding up Workbench considerably.

By default, this caching is enabled for requests that Workbench makes more than once and that are expected to have the same response each time, such as requests for field configurations or for checks for the existence of taxonomy terms.

Note that you should not normally have to disable this caching, but if you do (for example, if you are asked to during troubleshooting), you can do so by including the following setting in your configuration file:

```yaml
enable_http_cache: false
```

#### HTTP retries

Workbench retries failed HTTP requests of the "server failed to fulfil an apparently valid request" variety (response status codes 5xx). These retries are invisible and automatic, but if you suspect that the default values for the retry configuration are putting a burden on Drupal, you can configure the retry mechanism using the settings below, at the risk of increasing the probability that Workbench might encounter a fatal exception or time out:

- `http_max_retries`: Default is `3`; lowering it to 2 or 1 retries might reduce load on your Drupal server, depending on how often retries are necessary. Set to 0 to disable retries.
- `http_backoff_factor`: Default is `1`, which is (basically) the number of seconds between retries; increasing this value will likely reduce load on your Drupal server.
- `http_retry_on_status_codes`: Default is `[500, 502, 503, 504]`; removing one or more of these status code will not likely reduce load on your Drupal server. You probably don't need to change this setting.
- `http_retry_allowed_methods`: Default is `["HEAD", "GET", "POST", "PUT", "PATCH", "DELETE"]`; removing one or more of these HTTP methods will not likely reduce load on your Drupal server. You probably don't need to change this setting.

The only settings that may result in any appreciable reduction in load on your Drupal server are `http_backoff_factor` and `http_max_retries`. Generally speaking, however, changing the settings controlling HTTP request retries is not a recommended strategy for reducing load on your Web server, since any slight reduction in load on Drupal achieved by alterning these settings will be offset by an increased likelihood that Workbench will encounter an error.
