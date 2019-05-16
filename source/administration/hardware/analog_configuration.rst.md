# Analog card configuration

## Limitations

  - Wazo does not support hardware echocanceller on the TDM400 card.
    Users of TDM400 card willing to setup an echocanceller will have to
    use a software echocanceller like OSLEC.

## Verifications

Verify that one of the `{wctdm,wctdm24xxp}` module is uncommented in
<span data-role="file">/etc/dahdi/modules</span> depending on the card
you installed in your server.

If it wasn't, do again the step
<span data-role="ref">load\_dahdi\_modules</span>

<div class="note">

<div class="admonition-title">

Note

</div>

Analog cards work with card module. You must add the appropriate card
module to your analog card. Either:

  - an FXS module (for analog equipment - phones, ...),
  - an FXO module (for analog line)

</div>

## Generate DAHDI configuration

Issue the command:

    dahdi_genconf

<div class="warning">

<div class="admonition-title">

Warning

</div>

it will erase all existing configuration in
<span data-role="file">/etc/dahdi/system.conf</span> and
<span data-role="file">/etc/asterisk/dahdi-channels.conf</span> files \!

</div>

## Configure

### DAHDI system.conf configuration

First step is to check
<span data-role="file">/etc/dahdi/system.conf</span> file:

  - check the span numbering,

See detailed explanations of this file in the
<span data-role="ref">system\_conf</span> section.

Below is **an example** for a typical FXS analog line span:

    # Span 2: WCTDM/4 "Wildcard TDM400P REV I Board 5"
    fxoks=32
    echocanceller=mg2,32

### Asterisk dahdi-channels.conf configuration

Then you have to modify the
<span data-role="file">/etc/asterisk/dahdi-channels.conf</span> file:

  - remove the unused lines like:
    
        context = default
        group = 63

  - change the `context` and `callerid` lines if needed,

  - the `signalling` should be one of:
    
      - `fxo_ks` for **FXS** lines -yes it is the reverse
      - `fxs_ks` for **FXO** lines - yes it is the reverse

Below is **an example** for a typical french PRI line span:

    ; Span 2: WCTDM/4 "Wildcard TDM400P REV I Board 5"
    signalling=fxo_ks
    callerid="Channel 32" <4032>
    mailbox=4032
    group=5
    context=default
    channel => 32

## Next step

Now that you have configured your PRI card:

1.  you must check if you need to follow one of the
    <span data-role="ref">analog\_card\_specific\_conf</span> sections
    below,
2.  then, if you have another type of card to configure, you can go back
    to the <span data-role="ref">configure your card
    \<card\_configuration\></span> section,
3.  if you have configured all your card you have to configure the
    <span data-role="ref">interco\_dahdi\_conf</span> in the web
    interface.

## Specific configuration

### FXS modules

If you use **FXS** modules you should create the file
<span data-role="file">/etc/modprobe.d/xivo-tdm</span> and insert the
line:

    options DAHDI_MODULE_NAME fastringer=1 boostringer=1

Where DAHDI\_MODULE\_NAME is the DAHDI module name of your card (e.g.
wctdm for a TDM400P).

### FXO modules

If you use **FXO** modules you should create file
<span data-role="file">/etc/modprobe.d/xivo-tdm</span>:

    options DAHDI_MODULE_NAME opermode=FRANCE

Where DAHDI\_MODULE\_NAME is the DAHDI module name of your card (e.g.
wctdm for a TDM400P).
