# CSV User Import

Users and common related resources can be imported onto a Wazo server by
sending a CSV file with a predefined <span data-role="ref">set of fields
\<user\_import\></span>.

This page only documents additional notes useful for API users. Consult
the [API documentation](http://api.wazo.community) for more details.

## Uploading files

Files may be uploaded as usual through the web interface, or from a
console by using HTTP utilities and the REST API. When uploading through
the API, the header Content-Type: text/csv charset=utf-8 must be set and
the CSV data must be sent in the body of the request. A file may be
uploaded using curl as
follows:

``` sourceCode bash
curl -k -H "Content-Type: text/csv; charset=utf-8" -u username:password --data-binary "@file.csv" https://wazo:9486/1.1/users/import
```

The response can be reindented in a more readable format by piping the
output through python -m
json.tool in the following way:

``` sourceCode bash
curl (...) | python -m json.tool
```
