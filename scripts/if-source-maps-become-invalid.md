# If Source Maps Become Invalid

If you notice that the browser show incorrect location of source code in the `Developer Tools`, it means that the source map contains incorrect information about the source files. 

A source map file doesn't match the source files if its older version is used from the browser's cache. 

To resolve this issue, disable cache in the `Developer Tools -> Network` tab and refresh the page.  