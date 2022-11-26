## Reducing Workbench's impact on Drupal

Workbench can put substantial stress on Drupal. In some cases, this stress can lead to instability and errors. Workbench offers two main ways to reduce this stress.

The first is by telling Workbench to pause between each request it makes to Drupal. There are two types of pause, 1) basic pause and 2) adaptive pause.

!!! note
    Both types of pausing will slow down Workbench's overall execution time since they reduce speed to improve stability and reliability.

### Basic pause

The `pause` configuration setting tells Workbench to temporarily halt execution before every 'POST', 'PUT', 'PATCH', and 'DELETE' request, thereby spreading load caused by the requests over a longer period of time. To enable `pause`, include the setting in your configuration file, indicating the number of seconds to wait between requests:

```yaml
pause: 2
```

Using `pause` will help decrease load-induced errors, but it is inefficient because it causes Workbench to pause between *all* requests, even ones that are not putting stress on Drupal. A useful strategy for refining Workbench's load-reduction capabilities is to try `pause` first, and if it reduces errors, then disable `pause` and try `adaptive_pause` instead.

!!! note
    `pause` and `adaptive_pause` are mutually exclusive. If you include one in your configuration files, you should not include the other.

### Adaptive pause

Adaptive pause only halts execution between requests if Workbench detects that Drupal is slowing down. It does this by comparing Drupal's response time for the most recent request to the average response time of the 20 previous requests. If the response time for the most recent request reaches a specific threshold, Workbench's adaptive pause will kick in and temporarily halt execution to allow Drupal to catch up.

The threshold that needs to be met is configured using the `adaptive_pause_threshold` setting. This setting's default value is 2, which means that the adaptive pause will kick in if the response time for the most recent request Workbench makes to Drupal is 2 times (double) the average of the last 20 requests.

The amount of time that Workbench will pause is determined by the value of `adaptive_pause`, which, like the value for `pause`, is a number of seconds (e.g., `adaptive_pause: 3`). You enable adaptive pausing by adding the `adaptive_pause` setting to your configuration file. Since `adaptive_pause_threshold` has a default value (2) but `adaptive_pause` does not, you should only use `adaptive_pause_threshold` if you want to override the default value. Therefore, both of the following configurations are valid. The first enables `adaptive_pause` telling it to pause for 3 seconds between requests if Drupal's response time to the last request is 2 times slower than the average of the last 20 requests (using the default value of 2 `adaptive_pause_threshold`): 

```yaml
adaptive_pause: 3
```

In the next example, we override `adaptive_pause_threshold`'s default by including the setting in the configuration:

```yaml
adaptive_pause: 2
adaptive_pause_threshold: 2.5 
```

In this example, adaptive pausing only kicks in if the response time for the most recent request is 2.5 times the average of the response time for the last 20 requests. You can increment `adaptive_pause_threshold`'s value by .5 (e.g., 2.5, 3, 3.5, etc.) until you find a sweet spot that balances reliability with overall execution time. You can also decrease or increase the value of `adaptive_pause` incrementally by intervals of .5 to further refine the balance.

The number of previous requests used to determine the average response time is hard-coded to 20. There is no configuration option to change that number.

### Logging Drupal's response time

If a request if paused by adaptive pausing, Workbench will automatically log the response time for the next request, indicating that `adaptive_pause` has temporarily halted execution. If you want to log Drupal's response time regardless of whether `adaptive_pause` had kicked in or not, add `log_response_time: true` to your configuration file. All logging of response time includes variation from the average of the last 20 response times.

### Caching

The second way that Workbench reduces stress on Drupal is by caching HTTP requests. By default, this caching is enabled for requests that Workbench makes more than once and that are expected to have the same response each time, such as requests for field configurations or for checks for the existence of taxonomy terms.

This caching both reduces the load on Drupal and speeds up Workbench considerably. Note that you should not normally have to disable this caching, but if you do (for example, if you are asked to during troubleshooting), you can do so by including the following setting in your configuration file:

```yaml
enable_http_cache: false
```
