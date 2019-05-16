# Stock Plugins Documentation

## View Plugins

### default\_json

View name: default\_json

Purpose: present directory entries in JSON format. The format is
detailed in <http://api.wazo.community>.

### headers

View name: headers

Purpose: List headers that will be available in results from
`default_json` view.

### personal\_view

View name: personal\_view

Purpose: Expose REST API to manage personal contacts (create, delete,
list).

### phonebook\_view

View name: phonebook\_view

Purpose: Expose REST API to manage wazo-dird's internal phonebooks.

### aastra\_view

View name: aastra\_view

Purpose: Expose REST API to search in configured directories for Aastra
phone.

### cisco\_view

View name: cisco\_view

Purpose: Expose REST API to search in configured directories for Cisco
phone (see
[CiscoIPPhone\_XML\_Objects](http://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cuipph/all_models/xsi/8_5_1/xsi_dev_guide/xmlobjects.html)).

### polycom\_view

View name: polycom\_view

Purpose: Expose REST API to search in configured directories for Polycom
phone.

### snom\_view

View name: snom\_view

Purpose: Expose REST API to search in configured directories for Snom
phone.

### thomson\_view

View name: thomson\_view

Purpose: Expose REST API to search in configured directories for Thomson
phone.

### yealink\_view

View name: yealink\_view

Purpose: Expose REST API to search in configured directories for Yealink
phone.

## Service Plugins

### lookup

Service name: lookup

Purpose: Search through multiple data sources, looking for entries
matching a word.

#### Configuration

Example (excerpt from the main configuration file):

``` sourceCode yaml
services:
    lookup:
        default:
            sources:
                my_csv: true
            timeout: 0.5
```

The configuration is a dictionary whose keys are profile names and
values are configuration specific to that profile.

For each profile, the configuration keys are:

  - sources  
    The list of source names that are to be used for the lookup

  - timeout  
    The maximum waiting time for an answer from any source. Results from
    sources that take longer to answer are ignored. Default: no timeout.

### favorites

Service name: favorites

Purpose: Mark/unmark contacts as favorites and get the list of all
favorites.

### personal

Service name: personal

Purpose: Add, delete, list personal contacts of users.

### phonebook

Service name: phonebook

Purpose: Add, delete, list phonebooks and phonebook contacts.

#### Configuration

Example (excerpt from the main configuration file):

``` sourceCode yaml
services:
    favorites:
        default:
            sources:
                my_csv: true
            timeout: 0.5
```

The configuration is a dictionary whose keys are profile names and
values are configuration specific to that profile.

For each profile, the configuration keys are:

  - sources  
    The list of source names that are to be used for the lookup

  - timeout  
    The maximum waiting time for an answer from any source. Results from
    sources that take longer to answer are ignored. Default: no timeout.

### reverse

Service name: reverse

Purpose: Search through multiple data sources, looking for the first
entry matching an extension.

#### Configuration

Example:

``` sourceCode yaml
services:
    reverse:
        default:
            sources:
                my_csv: true
            timeout: 1
```

The configuration is a dictionary whose keys are profile names and
values are configuration specific to that profile.

For each profile, the configuration keys are:

  - sources  
    The list of source names that are to be used for the reverse lookup

  - timeout  
    The maximum waiting time for an answer from any source. Results from
    sources that take longer to answer are ignored. Default: 1.

### Service Discovery

Service name: service\_discovery

Purpose: Creates sources when services are registered using service
discovery.

To configure new sources, the service needs the following things:

1.  A template the for the source configuration file.
2.  A set of configuration that will be applied to the template.
3.  A set of service and profile that will use the new source.

<div class="note">

<div class="admonition-title">

Note

</div>

Service discovery is limited to a single service being discovered. This
means that discovering a xivo-confd server will assume that wazo-auth
resides on the same host or that the template is already configured with
the appropriate hostname.

</div>

#### Template

The template is used to generate the content of the configuration file
for the new service. Its content should be the same as the content of a
source for the desired backend.

The location of the templates are configured in the service
configuration

Example:

``` sourceCode yaml
type: wazo
name: wazo-{{ uuid }}
searched_columns:
- firstname
- lastname
first_matched_columns:
- exten
auth:
  host: {{ hostname }}
  port: 9497
  username: {{ service_id }}
  password: {{ service_key }}
  verify_certificate: false
confd:
  host: {{ hostname }}
  port: {{ port }}
  version: "1.1"
  verify_certificate: false
format_columns:
  name: "{firstname} {lastname}"
  phone: "{exten}"
  number: "{exten}"
  reverse: "{firstname} {lastname}"
  voicemail: "{voicemail_number}"
```

Example:

``` sourceCode yaml
services:
  service_discovery:
    template_path: /etc/wazo-dird/templates.d
    services:
      xivo-confd:
        template: confd.yml
```

In this example, the file */etc/wazo-dird/templates.d/confd.yml* would
be used to create a new source configuration when a new *xivo-confd*
service is registered.

The following keys are available to use in the templates:

  - uuid: The Wazo uuid that was in the service registry notification
  - hostname: The advertised host from the remote service
  - port: The advertised port from the remote service
  - service\_id: The login used to query xivo-confd
  - service\_key: The password used to query xivo-confd

All other fields are configured in the *hosts* section of the
service\_discovery service.

#### Host configuration

The host section allow the administrator to configure some information
that are not available in the service discovery to be available in the
templates. This will typically be the *service\_id* and *service\_key*
that are configured with the proper ACL on the remote Wazo.

Example:

``` sourceCode yaml
services:
  service_discovery:
    hosts:
      ff791b0e-3d28-4b4d-bb90-2724c0a248cb:
        uuid: ff791b0e-3d28-4b4d-bb90-2724c0a248cb
        service_id: some-service-name
        service_key: secre7
        datacenter: dc1
        token: 3f031816-84a6-3960-fcd1-9cca67eacde2
```

  - uuid: the XIVO\_UUID of the remote Wazo
  - service\_id: the web service login on the remote Wazo
  - service\_key: the secret key of the web service
  - datacenter(optional): the name of the consul datacenter on which the
    other Wazo is running
  - token(optional): the token to access service discovery on the remote
    consul

#### Profile and service association

The service and profile association for discovered services is defined
in the service\_discovery service configuration.

Example:

``` sourceCode yaml
services:
  service_discovery:
    services:
      xivo-confd:
        lookup:
          default: true
          foobar: true
        reverse:
          foobar: true
        favorites:
          default: true
          foobar: true
```

In this example, a new xivo-confd service would generate a configuration
based on the template and that new source would be added to the lookup
and favorites

## Back-end Configuration

This sections completes the
<span data-role="ref">dird-sources\_configuration</span> section.

### csv

Back-end name: csv

Purpose: read directory entries from a CSV file.

Limitations:

  - the CSV delimiter is not configurable (currently: `,` (comma)).

#### Configuration

Example (a file inside `source_config_dir`):

``` sourceCode yaml
type: csv
name: my_csv
file: /var/tmp/test.csv
unique_column: id
searched_columns:
    - fn
    - ln
first_matched_columns:
    - num
format_columns:
    lastname: "{ln}"
    firstname: "{fn}"
    number: "{num}"
```

With the CSV file:

``` sourceCode text
id,fn,ln,num
1,Alice,Abrams,55553783147
2,Bob,Benito,5551354958
3,Charles,Curie,5553132479
```

  - file  
    the absolute path to the CSV file

### CSV web service

Back-end name: csv\_ws

Purpose: search using a web service that returns CSV formatted results.

Given the following configuration, *wazo-dird* would call
"<https://example.com:8000/ws-phonebook?firstname=alice&lastname=alice>"
for a lookup for the term "alice".

#### Configuration

Example (a file inside `source_config_dir`):

``` sourceCode yaml
type: csv_ws
name: a_csv_web_service
lookup_url: "https://example.com:8000/ws-phonebook"
list_url: "https://example.com:8000/ws-phonebook"
verify_certificate: False
searched_columns:
  - firstname
  - lastname
first_matched_columns:
    - exten
delimiter: ","
timeout: 16
unique_column: id
format_columns:
    number: "{exten}"
```

  - lookup\_url  
    the URL used for directory searches.

  - list\_url (optional)  
    the URL used to list all available entries. This URL is used to
    retrieve favorites.

  - verify\_certificate (optional)  
    whether the SSL cert will be verified. A CA\_BUNDLE path can also be
    provided. Defaults to True.

  - delimiter (optional)  
    the field delimiter in the CSV result. Default: ','

  - timeout (optional)  
    the number of seconds before the lookup on the web service is
    aborted. Default: 10.

### dird\_phonebook

back-end name: dird\_phonebook

Purpose: search the wazo-dird's internal phonebooks

#### Configuration:

``` sourceCode yaml
type: dird_phonebook
name: phonebook
tenant: default
phonebook_id: 42
phonebook_name: main
first_matched_columns:
  - number
searched_columns:
  - firstname
  - lastname
format_columns:
    name: "{firstname} {lastname}"
```

  - tenant  
    the tenant of the phonebook to query.

  - phonebook\_name  
    the name of the phonebook used by this source.

  - phonebook\_id (deprecated, use phonebook\_name)  
    the id of the phonebook used by this source.

### ldap

Back-end name: ldap

Purpose: search directory entries from an LDAP server.

#### Configuration

Example (a file inside `source_config_dir`):

``` sourceCode yaml
type: ldap
name: my_ldap
ldap_uri: ldap://example.org
ldap_base_dn: ou=people,dc=example,dc=org
ldap_username: cn=admin,dc=example,dc=org
ldap_password: foobar
ldap_custom_filter: (l=québec)
unique_column: entryUUID
searched_columns:
    - cn
first_matched_columns:
    - telephoneNumber
format_columns:
    firstname: "{givenName}"
    lastname: "{sn}"
    number: "{telephoneNumber}"
```

  - ldap\_uri  
    the URI of the LDAP server. Can only contains the scheme, host and
    port part of an LDAP URL.

  - ldap\_base\_dn  
    the DN of the entry at which to start the search

  - ldap\_username (optional)  
    the user's DN to use when performing a "simple" bind.
    
    Default to an empty string.
    
    When both ldap\_username and ldap\_password are empty, an anonymous
    bind is performed.

  - ldap\_password (optional)  
    the password to use when performing a "simple" bind.
    
    Default to an empty string.

  - ldap\_custom\_filter (optional)  
    the custom filter is used to add more criteria to the filter
    generated by the back end.
    
    Example:
    
      - ldap\_custom\_filter: (l=québec)
      - searched\_columns: \[cn,st\]
    
    will result in the following filter being used for searches.
    `(&(l=québec)(|(cn=*%Q*)(st=*%Q*)))`
    
    If only the custom filter is to be used, leave the
    `searched_columns` field empty.
    
    This must be a valid [LDAP
    filter](https://tools.ietf.org/html/rfc4515), where the string `%Q`
    will be replaced by the (escaped) search term when performing a
    search.
    
    Example: `(&(o=ACME)(cn=*%Q*))`

  - ldap\_network\_timeout (optional)  
    the maximum time, in second, that an LDAP network operation can
    take. If it takes more time than that, no result is returned.
    
    Defaults to 0.3.

  - ldap\_timeout (optional)  
    the maximum time, in second, that an LDAP operation can take.
    
    Defaults to 1.0.

  - unique\_column (optional)  
    the column that contains a unique identifier of the entry. This is
    necessary for listing and identifying favorites.
    
    For OpenLDAP, you should set this option to "entryUUID".
    
    For Active Directory, you should set this option to "objectGUID" and
    also set the "unique\_column\_format" option to "binary\_uuid".

  - unique\_column\_format (optional)  
    the unique column's type returned by the queried LDAP server. Valid
    values are "string" or "binary\_uuid".
    
    Defaults to "string".

### personal

Back-end name: personal

Purpose: Add search results from the user personal contacts

#### Configuration

``` sourceCode yaml
type: personal
name: personal
first_matched_columns:
    - number
    - mobile
searched_columns:
    - firstname
    - lastname
    - mobile
    - number
format_columns:
    reverse: "{firstname} {lastname}"
    number: "{number}"
    phone_mobile: "{mobile}"
```

### wazo

Back-end name: wazo

Purpose: add users from a Wazo (may be remote) as directory entries

This backend requires a username and password that have the sufficient
permissions to list users and get the xivo-confd server info.

#### Required ACL

  - confd.users.read
  - confd.infos.read

#### Configuration

Example (a file inside `source_config_dir`):

``` sourceCode yaml
type: wazo
name: my_wazo
auth:
   host: wazo.example.com
   port: 9497
   username: admin
   password: password
   verify_certificate: "/usr/share/xivo-certs/server.crt"
confd:
    host: wazo.example.com
    port: 9486
    version: 1.1
    timeout: 3
    verify_certificate: "/usr/share/xivo-certs/server.crt"
unique_column: id
first_matched_columns:
    - exten
searched_columns:
    - firstname
    - lastname
format_columns:
    number: "{exten}"
    mobile: "{mobile_phone_number}"
```

  - auth:host  
    the hostname of the wazo-auth service

  - auth:port  
    the port of the wazo-auth service

  - auth:username  
    the username used to do queries on xivo-confd to search for users

  - confd:host  
    the hostname of xivo-confd service

  - confd:port  
    the port of the xivo-confd service (usually 9486)

  - confd:version  
    the version of the xivo-confd API (should be 1.1)
