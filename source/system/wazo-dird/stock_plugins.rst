.. _stock-plugins:

===========================
Stock Plugins Documentation
===========================

View Plugins
============

default_json
------------

View name: default_json

Purpose: present directory entries in JSON format. The format is detailed in http://api.wazo.community.

headers
-------

View name: headers

Purpose: List headers that will be available in results from ``default_json`` view.

personal_view
-------------

View name: personal_view

Purpose: Expose REST API to manage personal contacts (create, delete, list).

phonebook_view
--------------

View name: phonebook_view

Purpose: Expose REST API to manage wazo-dird's internal phonebooks.

aastra_view
-----------

View name: aastra_view

Purpose: Expose REST API to search in configured directories for Aastra phone.

cisco_view
----------

View name: cisco_view

Purpose: Expose REST API to search in configured directories for Cisco phone (see CiscoIPPhone_XML_Objects_).

.. _CiscoIPPhone_XML_Objects: http://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cuipph/all_models/xsi/8_5_1/xsi_dev_guide/xmlobjects.html

polycom_view
-------------

View name: polycom_view

Purpose: Expose REST API to search in configured directories for Polycom phone.

snom_view
---------

View name: snom_view

Purpose: Expose REST API to search in configured directories for Snom phone.

thomson_view
------------

View name: thomson_view

Purpose: Expose REST API to search in configured directories for Thomson phone.

yealink_view
------------

View name: yealink_view

Purpose: Expose REST API to search in configured directories for Yealink phone.


Service Plugins
===============

lookup
------

Service name: lookup

Purpose: Search through multiple data sources, looking for entries matching a word.

Configuration
^^^^^^^^^^^^^

Example (excerpt from the main configuration file):

.. code-block:: yaml
   :linenos:

   services:
       lookup:
           default:
               sources:
                   my_csv: true
               timeout: 0.5

The configuration is a dictionary whose keys are profile names and values are configuration specific
to that profile.

For each profile, the configuration keys are:

sources
   The list of source names that are to be used for the lookup

timeout
   The maximum waiting time for an answer from any source. Results from sources that take longer to
   answer are ignored. Default: no timeout.

favorites
---------

Service name: favorites

Purpose: Mark/unmark contacts as favorites and get the list of all favorites.


.. _dird_services_personal:

personal
--------

Service name: personal

Purpose: Add, delete, list personal contacts of users.


phonebook
---------

Service name: phonebook

Purpose: Add, delete, list phonebooks and phonebook contacts.


Configuration
^^^^^^^^^^^^^

Example (excerpt from the main configuration file):

.. code-block:: yaml
   :linenos:

   services:
       favorites:
           default:
               sources:
                   my_csv: true
               timeout: 0.5

The configuration is a dictionary whose keys are profile names and values are configuration specific
to that profile.

For each profile, the configuration keys are:

sources
   The list of source names that are to be used for the lookup

timeout
   The maximum waiting time for an answer from any source. Results from sources that take longer to
   answer are ignored. Default: no timeout.


reverse
-------

Service name: reverse

Purpose: Search through multiple data sources, looking for the first entry matching an extension.

Configuration
^^^^^^^^^^^^^

Example:

.. code-block:: yaml
   :linenos:

   services:
       reverse:
           default:
               sources:
                   my_csv: true
               timeout: 1

The configuration is a dictionary whose keys are profile names and values are configuration specific
to that profile.

For each profile, the configuration keys are:

sources
   The list of source names that are to be used for the reverse lookup

timeout
   The maximum waiting time for an answer from any source. Results from sources that take longer to
   answer are ignored. Default: 1.


Service Discovery
-----------------

Service name: service_discovery

Purpose: Creates sources when services are registered using service discovery.

To configure new sources, the service needs the following things:

#. A template the for the source configuration file.
#. A set of configuration that will be applied to the template.
#. A set of service and profile that will use the new source.


.. note:: Service discovery is limited to a single service being discovered. This means that discovering a xivo-confd server will assume that wazo-auth resides on the same host or that the template is already configured with the appropriate hostname.


Template
^^^^^^^^

The template is used to generate the content of the configuration file
for the new service. Its content should be the same as the content of a
source for the desired backend.

The location of the templates are configured in the service configuration

Example:

.. code-block:: yaml

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


Example:

.. code-block:: yaml

    services:
      service_discovery:
        template_path: /etc/wazo-dird/templates.d
        services:
          xivo-confd:
            template: confd.yml

In this example, the file */etc/wazo-dird/templates.d/confd.yml* would
be used to create a new source configuration when a new *xivo-confd* service
is registered.

The following keys are available to use in the templates:

* uuid: The Wazo uuid that was in the service registry notification
* hostname: The advertised host from the remote service
* port: The advertised port from the remote service
* service_id: The login used to query xivo-confd
* service_key: The password used to query xivo-confd

All other fields are configured in the *hosts* section of the service_discovery
service.


Host configuration
^^^^^^^^^^^^^^^^^^

The host section allow the administrator to configure some information that
are not available in the service discovery to be available in the templates.
This will typically be the *service_id* and *service_key* that are configured
with the proper ACL on the remote Wazo.

Example:

.. code-block:: yaml

    services:
      service_discovery:
        hosts:
          ff791b0e-3d28-4b4d-bb90-2724c0a248cb:
            uuid: ff791b0e-3d28-4b4d-bb90-2724c0a248cb
            service_id: some-service-name
            service_key: secre7
            datacenter: dc1
            token: 3f031816-84a6-3960-fcd1-9cca67eacde2

* uuid: the XIVO_UUID of the remote Wazo
* service_id: the web service login on the remote Wazo
* service_key: the secret key of the web service
* datacenter(optional): the name of the consul datacenter on which the other Wazo is running
* token(optional): the token to access service discovery on the remote consul


Profile and service association
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The service and profile association for discovered services is defined in the
service_discovery service configuration.

Example:

.. code-block:: yaml

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

In this example, a new xivo-confd service would generate a configuration based
on the template and that new source would be added to the lookup and favorites


Back-end Configuration
======================

This sections completes the :ref:`dird-sources_configuration` section.

.. _dird-backend-csv:

csv
---

Back-end name: csv

Purpose: read directory entries from a CSV file.

Limitations:

* the CSV delimiter is not configurable (currently: ``,`` (comma)).

Configuration
^^^^^^^^^^^^^

Example (a file inside ``source_config_dir``):

.. code-block:: yaml
   :linenos:

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

With the CSV file:

.. code-block:: text
   :linenos:

   id,fn,ln,num
   1,Alice,Abrams,55553783147
   2,Bob,Benito,5551354958
   3,Charles,Curie,5553132479


file
   the absolute path to the CSV file


.. _dird-backend-csv_ws:

CSV web service
---------------

Back-end name: csv_ws

Purpose: search using a web service that returns CSV formatted results.

Given the following configuration, *wazo-dird* would call
"https://example.com:8000/ws-phonebook?firstname=alice&lastname=alice" for a
lookup for the term "alice".


Configuration
^^^^^^^^^^^^^

Example (a file inside ``source_config_dir``):

.. code-block:: yaml
   :linenos:

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

lookup_url
    the URL used for directory searches.

list_url (optional)
    the URL used to list all available entries. This URL is used to retrieve favorites.

verify_certificate (optional)
    whether the SSL cert will be verified. A CA_BUNDLE path can also be provided. Defaults to True.

delimiter (optional)
    the field delimiter in the CSV result. Default: ','

timeout (optional)
    the number of seconds before the lookup on the web service is aborted. Default: 10.


.. _dird-backend-dird_phonebook:

dird_phonebook
--------------

back-end name: dird_phonebook

Purpose: search the wazo-dird's internal phonebooks

Configuration:
^^^^^^^^^^^^^^

.. code-block:: yaml
   :linenos:

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

tenant
    the tenant of the phonebook to query.

phonebook_name
    the `name` of the phonebook used by this source.

phonebook_id (deprecated, use phonebook_name)
    the `id` of the phonebook used by this source.


.. _dird-backend-ldap:

ldap
----

Back-end name: ldap

Purpose: search directory entries from an LDAP server.

Configuration
^^^^^^^^^^^^^

Example (a file inside ``source_config_dir``):

.. code-block:: yaml
   :linenos:

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


ldap_uri
   the URI of the LDAP server. Can only contains the scheme, host and port part of an LDAP URL.

ldap_base_dn
   the DN of the entry at which to start the search

ldap_username (optional)
   the user's DN to use when performing a "simple" bind.

   Default to an empty string.

   When both ldap_username and ldap_password are empty, an anonymous bind is performed.

ldap_password (optional)
   the password to use when performing a "simple" bind.

   Default to an empty string.

ldap_custom_filter (optional)
   the custom filter is used to add more criteria to the filter generated by the back end.

   Example:

   * ldap_custom_filter: (l=québec)
   * searched_columns: [cn,st]

   will result in the following filter being used for searches. ``(&(l=québec)(|(cn=*%Q*)(st=*%Q*)))``

   If only the custom filter is to be used, leave the ``searched_columns`` field
   empty.

   This must be a valid `LDAP filter <https://tools.ietf.org/html/rfc4515>`_, where the string ``%Q`` will be replaced by the (escaped) search
   term when performing a search.

   Example: ``(&(o=ACME)(cn=*%Q*))``

ldap_network_timeout (optional)
   the maximum time, in second, that an LDAP network operation can take. If it takes more time than
   that, no result is returned.

   Defaults to 0.3.

ldap_timeout (optional)
   the maximum time, in second, that an LDAP operation can take.

   Defaults to 1.0.

unique_column (optional)
   the column that contains a unique identifier of the entry. This is necessary for listing and
   identifying favorites.

   For OpenLDAP, you should set this option to "entryUUID".

   For Active Directory, you should set this option to "objectGUID" and also set the
   "unique_column_format" option to "binary_uuid".

unique_column_format (optional)
   the unique column's type returned by the queried LDAP server. Valid values are "string" or
   "binary_uuid".

   Defaults to "string".


personal
--------

Back-end name: personal

Purpose: Add search results from the user personal contacts


Configuration
^^^^^^^^^^^^^

.. code-block:: yaml
   :linenos:

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


.. _dird-backend-wazo:

wazo
----

Back-end name: wazo

Purpose: add users from a Wazo (may be remote) as directory entries

This backend requires a username and password that have the sufficient permissions
to list users and get the xivo-confd server info.


Required ACL
^^^^^^^^^^^^

* confd.users.read
* confd.infos.read


Configuration
^^^^^^^^^^^^^

Example (a file inside ``source_config_dir``):

.. code-block:: yaml
   :linenos:

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

auth:host
   the hostname of the wazo-auth service

auth:port
   the port of the wazo-auth service

auth:username
   the username used to do queries on xivo-confd to search for users

confd:host
   the hostname of xivo-confd service

confd:port
   the port of the xivo-confd service (usually 9486)

confd:version
   the version of the xivo-confd API (should be 1.1)
