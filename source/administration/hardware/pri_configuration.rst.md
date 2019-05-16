# PRI card configuration

## Verifications

Verify that the correct module is configured in
<span data-role="file">/etc/dahdi/modules</span> depending on the card
you installed in your server.

If it wasn't, do again the step
<span data-role="ref">load\_dahdi\_modules</span>

<div class="warning">

<div class="admonition-title">

Warning

</div>

*TE13x, TE23x, TE43x* cards :

  - these cards need a specific dahdi module configuration. See
    <span data-role="ref">dahdi\_linemode\_selection</span> paragraph,
  - you **MUST** install the correct echo-canceller firmware to be able
    to use these cards. See
    <span data-role="ref">hwec\_configuration</span> paragraph.

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
  - if needed change the clock source,
  - usually (at least in France) you should remove the `crc4`

See detailed explanations of this file in the
<span data-role="ref">system\_conf</span> section.

Below is **an example** for a typical french PRI line span:

    # Span 1: TE2/0/1 "T2XXP (PCI) Card 0 Span 1" CCS/HDB3/CRC4 RED
    span=1,1,0,ccs,hdb3
    # termtype: te
    bchan=1-15,17-31
    dchan=16
    echocanceller=mg2,1-15,17-31

### Asterisk dahdi-channels.conf configuration

Then you have to modify the
<span data-role="file">/etc/asterisk/dahdi-channels.conf</span> file:

  - remove the unused lines like:
    
        context = default
        group = 63

  - change the `context` lines if needed,

  - the `signalling` should be one of:
    
      - `pri_net`
      - `pri_cpe`

Below is **an example** for a typical french PRI line span:

    ; Span 1: TE2/0/1 "T2XXP (PCI) Card 0 Span 1" CCS/HDB3/CRC4 RED
    group = 0,11            ; belongs to group 0 and 11
    context = from-extern   ; incoming call to this span will be sent in 'from-extern' context
    switchtype = euroisdn
    signalling = pri_cpe    ; use 'pri_cpe' signalling
    channel => 1-15,17-31   ; the above configuration applies to channels 1 to 15 and 17 to 31

## Next step

Now that you have configured your PRI card:

1.  you must check if you need to follow one of the
    <span data-role="ref">pri\_card\_specific\_conf</span> sections
    below,
2.  then, if you have another type of card to configure, you can go back
    to the <span data-role="ref">configure your card
    \<card\_configuration\></span> section,
3.  if you have configured all your card you have to configure the
    <span data-role="ref">interco\_dahdi\_conf</span> in the web
    interface.

## Specific configuration

### Multiple PRI cards and sync cable

If you have several PRI cards in your server you should link them with a
synchronization cable to share the exact same clock.

To do this, you need to:

  - use the coding wheel on the Digium cards to give them an order of
    recognition in DAHDI/Asterisk (see
    [Digium\_telephony\_cards\_support](http://www.digium.com/en/support/telephony-cards)),
  - daisy-chain the cards with a sync cable (see
    [Digium\_telephony\_cards\_support](http://www.digium.com/en/support/telephony-cards)),
  - load the DAHDI module with the `timingcable=1` option.

Create
<span data-role="file">/etc/modprobe.d/xivo-timingcable.conf</span> file
and insert the line:

    options DAHDI_MODULE_NAME timingcable=1

Where `DAHDI_MODULE_NAME` is the DAHDI module name of your card (e.g.
wct4xxp for a TE205P).
