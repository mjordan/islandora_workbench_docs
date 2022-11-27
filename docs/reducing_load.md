Workbench can put substantial stress on Drupal. In some cases, this stress can lead to instability and errors.

!!! note
    The options described below modify Workbench's interaction with Drupal. They do not have a direct impact on the load experienced by the various microservices Islandora uses to index data in its triplestore, extract full text for indexing, generating thumbnails, etc. However, since Drupal is the "controller" for these microservices, reducing the number of Workbench requests on Drupal will also indirectly reduce the load experienced by Islandora's microservices. 

Workbench offers two main ways to reduce this stress: pausing Workbench between its requests to Drupal, and caching Workbench's requests to Drupal.

The first way to reduce stress on Drupal is by telling Workbench to pause between each request it makes. There are two types of pause, 1) basic pause and 2) adaptive pause. Both types of pausing will slow down Workbench's overall execution time since they reduce speed to improve stability and reliability.

### Basic pause

The `pause` configuration setting tells Workbench to temporarily halt execution before every 'POST', 'PUT', 'PATCH', and 'DELETE' request, thereby spreading load caused by the requests over a longer period of time. To enable `pause`, include the setting in your configuration file, indicating the number of seconds to wait between requests:

```yaml
pause: 2
```

Using `pause` will help decrease load-induced errors, but it is inefficient because it causes Workbench to pause between *all* requests, even ones that are not putting stress on Drupal. A useful strategy for refining Workbench's load-reduction capabilities is to try `pause` first, and if it reduces errors, then disable `pause` and try `adaptive_pause` instead.

!!! note
    `pause` and `adaptive_pause` are mutually exclusive. If you include one in your configuration files, you should not include the other.

### Adaptive pause

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

doesn't do anything. In other words, you can use `adaptive_pause` on its own, or you can use `adaptive_pause` and `adaptive_pause_threshold` together, but you should not use `adaptive_pause_threshold` on its own.

### Logging Drupal's response time

If a request if paused by adaptive pausing, Workbench will automatically log the response time for the next request, indicating that `adaptive_pause` has temporarily halted execution. If you want to log Drupal's response time regardless of whether `adaptive_pause` had kicked in or not, add `log_response_time: true` to your configuration file. All logging of response time includes variation from the average of the last 20 response times.

### Caching

The second way that Workbench reduces stress on Drupal is by caching HTTP requests. By default, this caching is enabled for requests that Workbench makes more than once and that are expected to have the same response each time, such as requests for field configurations or for checks for the existence of taxonomy terms.

This caching both reduces the load on Drupal and speeds up Workbench considerably. Note that you should not normally have to disable this caching, but if you do (for example, if you are asked to during troubleshooting), you can do so by including the following setting in your configuration file:

```yaml
enable_http_cache: false
```
