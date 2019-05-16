# Provd Management

## Get the Provd Manager

The provd manager resource represents the main entry point to the
xivo-provd REST API.

It links to the following resources:

  - The `dev` relation links to a <span data-role="ref">device manager
    \<provd-api-dev-mgr\></span>.
  - The `cfg` relation links to a <span data-role="ref">config manager
    \<provd-api-cfg-mgr\></span>.
  - The `pg` relation links to a <span data-role="ref">plugin manager
    \<provd-api-pg-mgr\></span>.
  - The `srv.configure` relation links to the provd manager
    <span data-role="ref">configuration service
    \<provd-api-configure\></span>.

### Query

``` sourceCode none
GET /provd
```

### Example request

``` sourceCode http
GET /provd HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "links": [
        {
            "href": "/provd/dev_mgr",
            "rel": "dev"
        },
        {
            "href": "/provd/cfg_mgr",
            "rel": "cfg"
        },
        {
            "href": "/provd/pg_mgr",
            "rel": "pg"
        },
        {
            "href": "/provd/configure",
            "rel": "srv.configure"
        }
    ]
}
```
