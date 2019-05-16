<div class="index">

interconnections/simonics

</div>

# Simon Telephonics

The following configuration is based on the example found
[here](http://support.simonics.com/support/solutions/articles/3000033840-asterisk-sip-conf)

  - username: `GV18005551212`
  - password: `password`
  - exten: `18005551212`
  - host: `gvgw.simonics.com`

## General SIP configuration

  - `PUT /asterisk/sip/general {..., "match_auth_username": "yes", ...}`

## Trunk settings

  - `POST /trunks {"context": "from-extern"}`
  - `POST /endpoints/sip {"username": "GV18005551212", "secret":
    "password", "type": "friend", "host": "gvgw.simonics.com",
    options=[["qualify", "yes"], ["callerid", "18005551212"]]`
  - `PUT /trunks/{trunk_id}/endpoints/sip/{sip_id}`
  - `POST /registers/sip {"auth_username": "GV18005551212",
    "auth_password": "password", "transport": "udp", "remote_host":
    "GV18005551212", "callback_extension": "18005551212"}`
  - `PUT /trunks/{trunk_id}/registers/sip/{sip_id}`

## Outgoing calls

See the <span data-role="ref">voip\_provider\_outcall</span> section.

## Incoming calls

See the <span data-role="ref">voip\_provider\_incall</span> section.
