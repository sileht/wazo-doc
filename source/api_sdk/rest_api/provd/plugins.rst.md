# Plugins Management

## Get the Plugin Manager

The plugin manager links to the following resources:

  - The `srv.install` relation links to the plugin manager
    <span data-role="ref">installation service
    \<provd-api-install\></span>. This installation service permits
    installing/uninstalling plugins.
  - The `pg.plugins` relation links to the <span data-role="ref">list of
    plugins \<provd-api-pg-plugins\></span>.
  - The `pg.reload` relation links to the <span data-role="ref">plugin
    reload service \<provd-api-pg-reload\></span>.

### Query

``` sourceCode none
GET /provd/pg_mgr
```

### Example request

``` sourceCode http
GET /provd/pg_mgr HTTP/1.1
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
            "href": "/provd/pg_mgr/install",
            "rel": "srv.install"
        },
        {
            "href": "/provd/pg_mgr/plugins",
            "rel": "pg.plugins"
        },
        {
            "href": "/provd/pg_mgr/reload",
            "rel": "pg.reload"
        }
    ]
}
```

## List Plugins

List the installed plugins.

If you want to install/uninstall plugins, you need to go trough the
plugin installation service.

### Query

``` sourceCode none
GET /provd/pg_mgr/plugins
```

### Example request

``` sourceCode http
GET /provd/pg_mgr/plugins HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "plugins": {
        "xivo-aastra-3.3.1-SP2": {
            "links": [
                {
                    "href": "/provd/pg_mgr/plugins/xivo-aastra-3.3.1-SP2",
                    "rel": "pg.plugin"
                }
            ]
        },
        "xivo-cisco-sccp-9.0.3": {
            "links": [
                {
                    "href": "/provd/pg_mgr/plugins/xivo-cisco-sccp-9.0.3",
                    "rel": "pg.plugin"
                }
            ]
        }
    }
}
```

## Get a Plugin

The plugin links to the following resources:

  - The `pg.info` relation links to the <span data-role="ref">plugin
    information \<provd-api-pg-info\></span>.
  - The `srv.install` relation links to the plugin
    <span data-role="ref">installation service
    \<provd-api-install\></span>. Plugins usually provided this service
    to install/uninstall firmware and language files.

### Query

``` sourceCode none
GET /provd/pg_mgr/plugins/<plugin_id>
```

### Example request

``` sourceCode http
GET /provd/pg_mgr/plugins/xivo-aastra-3.3.1-SP2 HTTP/1.1
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
            "href": "/provd/pg_mgr/plugins/xivo-aastra-3.3.1-SP2/info",
            "rel": "pg.info"
        },
        {
            "href": "/provd/pg_mgr/plugins/xivo-aastra-3.3.1-SP2/install",
            "rel": "srv.install"
        }
    ]
}
```

## Get Information of a Plugin

### Query

``` sourceCode none
GET /provd/pg_mgr/plugins/<plugin_id>/info
```

### Example request

``` sourceCode http
GET /provd/pg_mgr/plugins/xivo-aastra-3.3.1-SP2/info HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "plugin_info": {
        "capabilities": {
            "Aastra, 6730i, 3.3.1.5089": {
                "sip.lines": 6
            },
            "Aastra, 6731i, 3.3.1.2235": {
                "sip.lines": 6,
                "switchboard": true
            },
            "Aastra, 6735i, 3.3.1.5089": {
                "sip.lines": 9
            },
            "Aastra, 6737i, 3.3.1.5089": {
                "sip.lines": 9
            },
            "Aastra, 6739i, 3.3.1.2235": {
                "sip.lines": 9
            },
            "Aastra, 6753i, 3.3.1.2235": {
                "sip.lines": 9
            },
            "Aastra, 6755i, 3.3.1.2235": {
                "sip.lines": 9,
                "switchboard": true
            },
            "Aastra, 6757i, 3.3.1.2235": {
                "sip.lines": 9,
                "switchboard": true
            },
            "Aastra, 9143i, 3.3.1.2235": {
                "sip.lines": 9
            },
            "Aastra, 9480i, 3.3.1.2235": {
                "sip.lines": 9
            }
        },
        "description": "Plugin for Aastra 6730i, 6731i, 6735i, 6737i, 6739i, 6753i, 6755i, 6757i, 6757i CT, 9143i, 9480i, 9480i CT in version 3.3.1 SP2.",
        "version": "1.1"
    }
}
```

## Reload a Plugin

Reload the given plugin. This is mostly useful during plugin
development, after changing the code of the plugin, instead of
restarting the xivo-provd application.

### Query

``` sourceCode none
POST /provd/pg_mgr/reload
```

### Example request

``` sourceCode http
POST /provd/pg_mgr/reload HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{
    "id": "xivo-aastra-3.3.1-SP2"
}
```

### Example response

``` sourceCode http
HTTP/1.1 204 No Content
```
