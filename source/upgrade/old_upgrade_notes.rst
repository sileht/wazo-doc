**********************
Archived Upgrade Notes
**********************


2015
====

15.20
-----

Consult the `15.20 Roadmap <https://projects.wazo.community/versions/214>`_

* Debian has been upgraded from version 7 (wheezy) to 8 (jessie).
* CSV webservices in the web interface have been removed. Please use the :ref:`confd-api` instead.
* The CSV import format has been changed. Consult :ref:`15_20_csv_import_upgrade_notes` for further details.
* xivo-ctid now uses STARTTLS for the client connections.

  * For users already using the CTIS protocol the client can be configured to use the default port (5003)

Please consult the following detailed upgrade notes for more information:

.. toctree::
   :maxdepth: 1

   15.20/jessie
   15.20/csv


15.19
-----

Consult the `15.19 Roadmap <https://projects.wazo.community/versions/236>`_

* The sound file :file:`/usr/share/asterisk/sounds/fr_FR/une.wav` has been moved to
  :file:`/usr/share/asterisk/sounds/fr_FR/digits/1F.wav`.
* If you would like to use the new `"transfer to voicemail" feature <http://projects.wazo.community/issues/5905>`_
  from the People Xlet, you'll need to update your directory definition and your directory display, i.e.:

  * edit your "internal" directory definition (Services / CTI server / Directories / Definitions)
    and add a field "voicemail" with value "voicemail_number"
  * edit your display (Services / CTI server / Directories / Display filters) and add a row with title
    "Voicemail", field type "voicemail" and field name "voicemail"
  * restart xivo-dird

* It is now possible to send an email to a user with a configured email address in the
  *people* xlet. See dird-integration-views to add the appropriate field to your
  configured displays.
* The *Contacts* xlet (aka. *Search*) has been removed in favor of the people-xlet. You may
  need to do some manual configuration in the directories for the People Xlet to be fully
  functional. See :ref:`the detailed upgrade notes <15_19_people_xlet_upgrade_notes>` for more details.
* If you need context separation in the People Xlet, you will have to **manually configure**
  xivo-dird to keep it working, see :ref:`15-19-dird-context-separation`. This procedure is only
  temporary, later versions will handle the context separation automatically.
* xivo-agentd now uses mandatory token authentication for its REST API. If you have custom
  development using this service, update your program accordingly.
* Some actions that used to be available in the *contact* xlets are not
  implemented in the *people* xlet yet.

  * Cancel transfer is only available using the *switchboard* xlet
  * Hanging up a call is only possible using the *switchboard* xlet
  * Call interception is not available anymore
  * Conference room invitation is not available anymore

Please consult the following detailed upgrade notes for more information:


.. toctree::
   :maxdepth: 1

   15.19/people-xlet-directory
   15.19/context-separation


15.18
-----

Consult the `15.18 Roadmap <https://projects.wazo.community/versions/234>`_

* The provd_pycli command (deprecated in 15.06) has been removed in favor of xivo-provd-cli. If you
  have custom scripts referencing provd_pycli, you'll need to update them.
* The xivo-agentctl command (deprecated in 15.06) has been removed in favor of xivo-agentd-cli. If you
  have custom scripts referencing xivo-agentctl, you'll need to update them.
* xivo-agentd now uses HTTPS. If you have custom development using this service, update your
  configuration accordingly. The xivo-agentd-client library, used to interact with xivo-agentd, has
  also been updated to use HTTPS by default.
* xivo-confd ports 50050 and 50051 have been removed. Please use 9486 and 9487 instead

**Configuration File Upgrade Notes**

The file format of configuration files for daemons exposing an HTTP/S API has changed.  The
following services have been affected :

 * xivo-agentd
 * xivo-amid
 * xivo-auth
 * xivo-confd
 * xivo-ctid
 * xivo-dird
 * xivo-dird-phoned

Ports and listening addresses are now organised in the following fashion:

.. code-block:: yaml

    rest_api:
      https:
        enabled: true
        port: 9486
        listen: 0.0.0.0
        certificate: /usr/share/xivo-certs/server.crt
        private_key: /usr/share/xivo-certs/server.key
        ciphers: "ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:!SEED:+HIGH:+MEDIUM"
      http:
        enabled: true
        port: 9487
        listen: 127.0.0.1

If you have any custom configuration files for these daemons, please modify them accordingly.
Consult :ref:`network_ports` for further details on which network services are available for each
daemon.


15.17
-----

Consult the `15.17 Roadmap <https://projects.wazo.community/versions/233>`_

* Online call recording is now done via `automixmon
  <https://wiki.asterisk.org/wiki/display/AST/One-Touch+Features>`_ instead of automon. This has no
  impact unless you have custom dialplan that is passing directly the "w" or "W" option to the Dial
  or Queue application. In these cases, you should modify your dialplan to pass the "x" or "X"
  option instead.
* The remote directory service available from :ref:`supported phones <supported-devices>` is now
  provided by the new unified directory service, i.e. xivo-dird. Additional upgrade steps are
  required to get the full benefit of the new directory service; see the :ref:`detailed upgrade
  notes <upgrade-notes-webi-to-dird>`.
* The field ``enableautomon`` has been renamed to ``enableonlinerec`` in the users web services provided
  by the web-interface (these web services are deprecated).
* The agent status dashboard now shows that an agent is calling or receiving a non ACD call while in
  wrapup or paused.
* SIP endpoints created through the REST API will not appear in the web interface until they have
  been associated with a line
* Due to limitations in the database, only a limited number of optional parameters can be configured
  on a SIP endpoint. Consult the :ref:`xivo-confd changelog <rest-api_changelog>` for further details


Please consult the following detailed upgrade notes for more information:

.. toctree::
   :maxdepth: 1

   15.17/webi_to_dird


15.16
-----

Consult the `15.16 Roadmap <https://projects.wazo.community/versions/232>`_

* The directory column type "mobile" was removed in favor of the new "callable" type. If you have
  hand-written configuration files for xivo-dird, in section "views", subsection "displays", all
  keys "type" with value "mobile" must be changed to value "callable".
* The ``xivo-auth`` backend interface has changed, ``get_acls`` is now ``get_consul_acls``. All
  unofficial back ends must be adapted and updated. No action is required for "normal" installations.
* Voicemails can now be deleted even if they are associated to a user.


.. _upgrade_notes_15_15:

15.15
-----

Consult the `15.15 Roadmap <https://projects.wazo.community/versions/231>`_

**Voicemail Upgrade Notes**

 * Voicemail webservices in the web interface have been removed. Please use the :ref:`confd-api` instead.
 * Voicemail IMAP configuration has been migrated to the new ``Advanced`` tab.
 * Voicemail option ``Disable password checking`` has been converted to ``Ask password``. The value has also been
   inverted. (e.g. If ``Disable password checking`` was false, ``Ask password`` is true.) ``Ask password`` is activated
   by default.
 * After an upgrade, if ever you have errors when searching for voicemails, please try clearing cookies in your web browser.
 * A voicemail must be dissociated from any user prior to being deleted. Voicemail are dissociated by editing the
   user and clicking on the ``Delete voicemail`` button in the ``Voicemail`` tab. This constraint will disappear in
   future versions.
 * Deleting a user will dissociate any voicemail that was attached, but will not delete it nor any messages.
 * Creating a line is no longer necessary when attaching a voicemail to a user.
 * The following fields have been modified when importing a CSV file:

+--------------------+----------------------+------------+-------------------+
| Old name           | New name             | Required ? | New default value |
+====================+======================+============+===================+
| voicemailmailbox   | voicemailnumber      | yes        |                   |
+--------------------+----------------------+------------+-------------------+
| voicemailskippass  | voicemailaskpassword | no         | 1                 |
+--------------------+----------------------+------------+-------------------+
|                    | voicemailcontext     | yes        |                   |
+--------------------+----------------------+------------+-------------------+

**Directories**

 * Concatenated fields in directories are now done in the directory definitions instead of the displays
 * The field column in directory displays are now field names from the directory definition. No more `{db-*}` are required
 * In the directory definitions fields can be modified using a python format string with the fields comming from the source.
 * Most of the configuration for xivo-dird is now generated from xivo-confgen using the values in the web interface.
 * The `remote directory` xlet has been removed in favor of the new `people` xlet.

See `wazo-dird-integration` for more details


15.14
-----

* Consult the `15.14 Roadmap <https://projects.wazo.community/versions/230>`_
* Default password for ``xivo-polycom-4.0.4`` plugin version >= 1.3 is now **9486** (i.e. the word
  "xivo" on a telephone keypad).
* Default password for ``xivo-polycom-5.3.0`` plugin version >= 1.4 is now **9486**.
* Caller id management for users in confd has changed. Consult the :ref:`xivo-confd changelog <rest-api_changelog>`.
* The Local Directory Xlet is replaced with the People Xlet. Contacts are automatically migrated to
  the server. Note that the CSV format for importing contacts has changed.

15.13
-----

* Consult the `15.13 Roadmap <https://projects.wazo.community/versions/229>`_
* Asterisk has been upgraded from version 11.17.1 to 13.4.0, which is a major Asterisk upgrade.
* An `ARI <https://wiki.asterisk.org/wiki/display/AST/Getting+Started+with+ARI>`_ user has been
  added to :file:`/etc/asterisk/ari.conf`. If you have configured Asterisk HTTP server to bind on a
  publicly reachable address (in :file:`/etc/asterisk/http.conf`), then you should update your
  configuration to prevent unauthorized access on your Asterisk.
* The xivo-dird configuration option `source_to_display_columns` has been
  removed in favor of the new option `format_columns`. All source configuration
  using the `source_to_display_columns` must be updated. A migration script will
  automatically modify source configuration in the `/etc/xivo-dird/sources.d`
  directory.

Please consult the following detailed upgrade notes for more information:

.. toctree::
   :maxdepth: 1

   15.13/asterisk_13


.. _upgrade-note-15.12:

15.12
-----

* Consult the `15.12 Roadmap <https://projects.wazo.community/versions/228>`_
* The certificate used for HTTPS in the web interface will be regenerated if the default certificate
  was used. Your browser will complain about the new certificate, and it is safe to accept it (see
  `#3656`_). See also :ref:`https_certificate`.
* If you have an :ref:`HA configuration <high-availability>`, then you should run ``xivo-sync -i`` on
  the master node to setup file synchronization between the master and the slave. File synchronization
  will then be done automatically every hour via rsync and ssh.
* xivo-auth and xivo-dird now use HTTPS, if you have custom development using these services, update
  your configuration accordingly.

.. _#3656: http://projects.wazo.community/issues/5636


15.11
-----

* Consult the `15.11 Roadmap <https://projects.wazo.community/versions/227>`_
* The call records older than 365 days will be periodically removed. The first automatic purge will
  occur in the night after the upgrade. See :ref:`purge_logs` for more details.


15.10
-----

* Consult the `15.10 Roadmap <https://projects.wazo.community/versions/223>`_


15.09
-----

* Consult the `15.09 Roadmap <https://projects.wazo.community/versions/226>`_


15.08
-----

* Consult the `15.08 Roadmap <https://projects.wazo.community/versions/225>`_
* The Dialer Xlet has been integrated in Identity Xlet.


15.07
-----

* Consult the `15.07 Roadmap <https://projects.wazo.community/versions/224>`_


15.06
-----

* Consult the `15.06 Roadmap <https://projects.wazo.community/versions/222>`_
* The provd client has been moved into a new python package, xivo_provd_client. If you have custom scripts
  using this client, you'll need to update them. See http://projects.wazo.community/issues/5469 for more
  information.
* The provd_pycli command name has been deprecated in favor of xivo-provd-cli. These 2 commands do the
  same thing, the only difference being the name of the command. The provd_pycli command name will be
  removed in 15.18, so if you have custom scripts referencing provd_pycli, you'll need to update them.
* The xivo-agentctl command name has been deprecated in favor of xivo-agentd-cli. These 2 commands do the
  same thing, the only difference being the name of the command. The xivo-agentctl command name will be
  removed in 15.18, so if you have custom scripts referencing xivo-agentctl, you'll need to update them.


15.05
-----

* Consult the `15.05 Roadmap <https://projects.wazo.community/versions/221>`_
* The Xlet identity has been modified to follow the new Wazo Client design which implies the removal
  of some details.


15.04
-----

* Consult the `15.04 Roadmap <https://projects.wazo.community/versions/220>`_


15.03
-----

* Consult the `15.03 Roadmap <https://projects.wazo.community/versions/219>`_


15.02
-----

* Consult the `15.02 Roadmap <https://projects.wazo.community/versions/218>`_


15.01
-----

* Consult the `15.01 Roadmap <https://projects.wazo.community/versions/217>`_

* The :ref:`confd REST API <confd-api>` is now more restrictive on HTTP headers. Particularly, the
  headers Accept and Content-Type must be set to (typically) ``application/json``.
* The following configuration files have been created:

  * :file:`/etc/xivo-agid/config.yml`
  * :file:`/etc/xivo-call-logd/config.yml`
  * :file:`/etc/xivo-amid/config.yml`
  * :file:`/etc/xivo-agentd/config.yml`
