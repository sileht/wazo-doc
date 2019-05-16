# General Resources

This section describes the resources that are available from more than
one URI or are generic enough to not fit in a more specific section.

## Operation In Progress

This resource represents an operation in progress and is used to follow
the progress of an underlying operation. Said differently, it is a
monitor on an operation that can change over time.

### Get Current Status

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

``` sourceCode http
GET <uri> HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "status": "progress"
}
```

The `status` field describe the current status of the operation. The
format is `[label|]state[;current[/end]](\(sub_oips\))*`. Here's some
examples:

  - progress
  - download|progress
  - download|progress;10
  - download|progress;10/100
  - downloadprogress;20/100)(file\_2|waiting;0/50)
  - downloadprogress)(file\_2|waiting)
  - opprogress(op11waiting))(op2|progress)

The state of an operation is either `waiting`, `progress`, `success` or
`fail`.

### Delete

Delete the "operation in progress" resource.

This does not cancel the underlying operation; it only deletes the
monitor. Every monitor that is created should be deleted, else they
won't be freed by the process and they will accumulate, taking memory.

#### Query

``` sourceCode none
DELETE <uri>
```

#### Example request

``` sourceCode http
DELETE <uri> HTTP/1.1
Host: wazoserver
```

#### Example response

``` sourceCode http
HTTP/1.1 204 No Content
```

## Configuration Service

### Get the Configuration

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

Example request for the configuration service of the
<span data-role="ref">provd manager \<provd-api-provd-mgr\></span>.

``` sourceCode http
GET /provd/configure HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "params": [
        {
            "description": "The plugins repository URL",
            "id": "plugin_server",
            "links": [
                {
                    "href": "/provd/configure/plugin_server",
                    "rel": "srv.configure.param"
                }
            ],
            "value": "http://provd.wazo.community/plugins/1/stable"
        },
        {
            "description": "The proxy for HTTP requests. Format is \"http://[user:password@]host:port\"",
            "id": "http_proxy",
            "links": [
                {
                    "href": "/provd/configure/http_proxy",
                    "rel": "srv.configure.param"
                }
            ],
            "value": null
        },
        {
            "description": "The proxy for FTP requests. Format is \"http://[user:password@]host:port\"",
            "id": "ftp_proxy",
            "links": [
                {
                    "href": "/provd/configure/ftp_proxy",
                    "rel": "srv.configure.param"
                }
            ],
            "value": null
        },
        {
            "description": "The proxy for HTTPS requests. Format is \"host:port\"",
            "id": "https_proxy",
            "links": [
                {
                    "href": "/provd/configure/https_proxy",
                    "rel": "srv.configure.param"
                }
            ],
            "value": null
        },
        {
            "description": "The current locale. Example: fr_FR",
            "id": "locale",
            "links": [
                {
                    "href": "/provd/configure/locale",
                    "rel": "srv.configure.param"
                }
            ],
            "value": null
        },
        {
            "description": "Set to 1 if all the devices are behind a NAT.",
            "id": "NAT",
            "links": [
                {
                    "href": "/provd/configure/NAT",
                    "rel": "srv.configure.param"
                }
            ],
            "value": 0
        }
    ]
}
```

### Get the Value of a Parameter

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

Example request for the NAT option of the configuration service of the
provd entry point.

``` sourceCode http
GET /provd/configure/NAT HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

    HTTP/1.1 200 OK
    Content-Type: application/vnd.proformatique.provd+json
    
    {
        "param": {
            "value": 0
        }
    }

### Set the Value of a Parameter

#### Query

``` sourceCode none
PUT <uri>
```

#### Example request

Example request for the NAT option of the configuration service of the
<span data-role="ref">provd manager
\<provd-api-provd-mgr\></span>.

``` sourceCode http
PUT /provd/configure/NAT HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{
    "param": {
        "value": 1
    }
}
```

#### Example response

``` sourceCode http
HTTP/1.1 204 No Content
Content-Type: application/vnd.proformatique.provd+json
```

## Installation Service

### Get the Installation Service

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

Example request for the installation service of the
<span data-role="ref">plugin manager \<provd-api-pg-mgr\></span>.

``` sourceCode http
GET /provd/pg_mgr/install HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "links": [
        {
            "href": "/provd/pg_mgr/install/install",
            "rel": "srv.install.install"
        },
        {
            "href": "/provd/pg_mgr/install/uninstall",
            "rel": "srv.install.uninstall"
        },
        {
            "href": "/provd/pg_mgr/install/installed",
            "rel": "srv.install.installed"
        },
        {
            "href": "/provd/pg_mgr/install/installable",
            "rel": "srv.install.installable"
        },
        {
            "href": "/provd/pg_mgr/install/upgrade",
            "rel": "srv.install.upgrade"
        },
        {
            "href": "/provd/pg_mgr/install/update",
            "rel": "srv.install.update"
        }
    ]
}
```

The upgrade and update services are optional and not all installation
service provide them.

### Install a Package

#### Query

``` sourceCode none
POST <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
POST /provd/pg_mgr/install/install HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{
    "id": "xivo-polycom-4.0.4"
}
```

#### Example response

``` sourceCode http
HTTP/1.1 201 Created
Location: /provd/pg_mgr/install/install/1
Content-Type: application/vnd.proformatique.provd+json
```

The URI returned in the `Location` header points to an
<span data-role="ref">operation in progress \<provd-api-oip\></span>
resource.

### Uninstall a Package

#### Query

``` sourceCode none
POST <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
POST /provd/pg_mgr/install/uninstall HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{
    "id": "xivo-polycom-4.0.4"
}
```

#### Example response

``` sourceCode http
HTTP/1.1 204 No Content
Content-Type: application/vnd.proformatique.provd+json
```

### Upgrade a Package

#### Query

``` sourceCode none
POST <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
POST /provd/pg_mgr/install/upgrade HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{
    "id": "xivo-polycom-4.0.4"
}
```

#### Example response

``` sourceCode http
HTTP/1.1 201 Created
Location: /provd/pg_mgr/install/upgrade/1
Content-Type: application/vnd.proformatique.provd+json
```

The URI returned in the `Location` header points to an
<span data-role="ref">operation in progress \<provd-api-oip\></span>
resource.

### Update the List of Installable Packages

#### Query

``` sourceCode none
POST <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
POST /provd/pg_mgr/install/update HTTP/1.1
Host: wazoserver
Content-Type: application/vnd.proformatique.provd+json

{}
```

#### Example response

``` sourceCode http
HTTP/1.1 201 Created
Location: /provd/pg_mgr/install/update/1
Content-Type: application/vnd.proformatique.provd+json
```

The URI returned in the `Location` header points to an
<span data-role="ref">operation in progress \<provd-api-oip\></span>
resource.

### List Installable Packages

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
GET /provd/pg_mgr/install/installable HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "pkgs": {
        "null": {
            "capabilities": {
                "*, *, *": {
                    "sip.lines": 0
                }
            },
            "description": "Plugin that offers no configuration service and rejects TFTP/HTTP requests.",
            "dsize": 1073,
            "sha1sum": "90b2fb6c2b135a9d539488b6a85779dd95e0e876",
            "version": "1.0"
        },
        "xivo-aastra-3.3.1-SP2": {
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
            "dsize": 9397,
            "sha1sum": "68dbed6afa87cf624a89166bdc6bdf7413cb84df",
            "version": "1.1"
        }
    }
}
```

### List Installed Packages

#### Query

``` sourceCode none
GET <uri>
```

#### Example request

Example request for the installation service of the plugin manager.

``` sourceCode http
GET /provd/pg_mgr/install/installed HTTP/1.1
Host: wazoserver
Accept: application/vnd.proformatique.provd+json
```

#### Example response

``` sourceCode http
HTTP/1.1 200 OK
Content-Type: application/vnd.proformatique.provd+json

{
    "pkgs": {
        "xivo-aastra-3.3.1-SP2": {
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
}
```
