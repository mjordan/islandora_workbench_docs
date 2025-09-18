### Using session cookies to access remote files

By default, any remote files named in your input CSV need to be accessible to the workbench script. However, it is possible to configure Workbench to use session cookies to use session cookies provided to it to authenticate against remote websites hosting the files you want to retrieve. This feature is intended for use on [Drupal / Islandora 7 websites](https://github.com/mjordan/islandora_workbench/issues/963), but may work on other sites that use standard browser cookies to maintain authentication state as well.

To configure Workbench to use a session cookie in this way, you need to 1) generate a cookie in your web browser, and 2) register the cookie in the Workbench configuration file.

#### Generating the cookie

First, log into the target website as usual. Then, copy the cookie data. Steps to do this depend on what web browser you used to log in with. `remote_file_cookie_name` and `remote_file_cookie_value` mentioned below are Workbench configuration settings that must be present in order for Workbench to access the remote files.

- Chrome: Right-click anywhere on the page > Inspect > click the Application tab > click on "Cookies" in the "Storage" section in the left sidebar. Then click on the website URL. You'll see a cookie in the table that starts with "SSESS...". Click on it and copy it; that's the remote_file_cookie_name. When you click on it, underneath you'll see a "Cookie Value" with a long hash. Copy that hash, and that is the `remote_file_cookie_value`.
- Firefox: Right-click anywhere on the page > Inspect > click the Storage tab > click on "Cookies". Click on the website URL under "Cookies." In the table below, click on the cookie that starts with "SSESS..." You will see a section of data below the table. The bold pink part is the remote_file_cookie_name. The hash to the right of it is the `remote_file_cookie_value`.
- Safari: Right-click anywhere on the page > Inspect Element > click the Storage tab > click on the "Cookies" folder and then on the website URL. In the table you'll see a cookie that starts with "SSESS..." Copy the part that's in the "Name" column of the table; that is the `remote_file_cookie_name`. Copy the part that's in the "Value" column of the table; that is the `remote_file_cookie_value`.

#### Register the cookie with Workbench

Add `remote_file_cookie_name` and `remote_file_cookie_value` to your configuration file as described above.

When you run Workbench, it will be able to authenticate against the remote site and fetch the files in the same way that it would fetch openly accessible files.

