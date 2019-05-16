# BRI card configuration

## Verifications

Verify that the `wcb4xxp` module is uncommented in
<span data-role="file">/etc/dahdi/modules</span>.

If it wasn't, do again the step
<span data-role="ref">load\_dahdi\_modules</span>.

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

See detailed explanations of this file in the
<span data-role="ref">system\_conf</span> section.

Below is **an example** for a typical french BRI line span:

    # Span 1: B4/0/1 "B4XXP (PCI) Card 0 Span 1" (MASTER) RED
    span=1,1,0,ccs,ami
    # termtype: te
    bchan=1-2
    hardhdlc=3
    echocanceller=mg2,1-2

### Asterisk dahdi-channels.conf configuration

Then you have to modify the
<span data-role="file">/etc/asterisk/dahdi-channels.conf</span> file:

  - remove the unused lines like:
    
        context = default
        group = 63

  - change the `context` lines if needed,

  - the `signalling` should be one of:
    
      - `bri_net`
      - `bri_cpe`
      - `bri_net_ptmp`
      - `bri_cpe_ptmp`

See some explanations of this file in the
<span data-role="ref">asterisk\_dahdi\_channel\_conf</span> section.

Below is **an example** for a typical french BRI line span:

    ; Span 1: B4/0/1 "B4XXP (PCI) Card 0 Span 1" (MASTER) RED
    group = 0,11              ; belongs to group 0 and 11
    context = from-extern     ; incoming call to this span will be sent in 'from-extern' context
    switchtype = euroisdn
    signalling = bri_cpe    ; use 'bri_cpe' signalling
    channel => 1-2          ; the above configuration applies to channels 1 and 2

## Next step

Now that you have configured your BRI card:

1.  you must check if you need to follow one of the
    <span data-role="ref">bri\_card\_specific\_conf</span> sections
    below,
2.  then, if you have another type of card to configure, you can go back
    to the <span data-role="ref">configure your card
    \<card\_configuration\></span> section,
3.  if you have configured all your card you have to configure the
    <span data-role="ref">interco\_dahdi\_conf</span> in the web
    interface.

## Specific configuration

You will find below 3 configurations that we recommend for BRI lines.
These configurations were tested on different type of french BRI lines
with success.

<div class="note">

<div class="admonition-title">

Note

</div>

The pre-requisites are:

  - Use per-port dahdi interconnection (see the
    <span data-role="ref">interco\_dahdi\_conf</span> section)

</div>

If you don't know which one to configure we recommend that you try each
one after the other in this order:

1.  <span data-role="ref">bri\_card\_ptmp\_wol1l2</span>
2.  <span data-role="ref">bri\_card\_ptmp\_wl1l2</span>
3.  <span data-role="ref">bri\_card\_ptp\_wl1l2</span>

### PTMP without layer1/layer2 persistence

In this mode we will configure asterisk and DAHDI:

  - to use Point-to-Multipoint (PTMP) signalling,
  - and to leave Layer1 and Layer2 DOWN

Follow theses steps to configure:

1.  **Before** the line `#include dahdi-channels.conf` add, in file
    <span data-role="file">/etc/asterisk/chan\_dahdi.conf</span>, the
    following lines:
    
        layer1_presence = ignore
        layer2_persistence = leave_down

2.  In the file
    <span data-role="file">/etc/asterisk/dahdi-channels.conf</span> use
    `bri_cpe_ptmp` signalling:
    
        signalling = bri_cpe_ptmp

3.  Create the file
    <span data-role="file">/etc/modprobe.d/xivo-wcb4xxp.conf</span> to
    deactivate the layer1 persistence:
    
        touch /etc/modprobe.d/xivo-wcb4xxp.conf

4.  Fill it with the following content:
    
        options wcb4xxp persistentlayer1=0

5.  Then, apply the configuration by restarting the services:
    
        wazo-service restart

<div class="note">

<div class="admonition-title">

Note

</div>

Expected behavior:

  - The dahdi show status command should show the BRI spans in *RED*
    status if there is no call,
  - For outgoing calls the layer1/layer2 should be brought back up by
    the Wazo (i.e. asterisk/chan\_dahdi),
  - For incoming calls the layer1/layer2 should be brought back up by
    the operator,
  - You can consider that there is *a problem* only if incoming or
    outgoing calls are rejected.

</div>

### PTMP with layer1/layer2 persistence

In this mode we will configure asterisk and DAHDI:

  - to use Point-to-Multipoint (PTMP) signalling,
  - and to keep Layer1 and Layer2 UP

Follow theses steps to configure:

1.  **Before** the line `#include dahdi-channels.conf` add, in file
    <span data-role="file">/etc/asterisk/chan\_dahdi.conf</span>, the
    following lines:
    
        layer1_presence = required
        layer2_persistence = keep_up

2.  In the file
    <span data-role="file">/etc/asterisk/dahdi-channels.conf</span> use
    `bri_cpe_ptmp` signalling:
    
        signalling = bri_cpe_ptmp

3.  If it exists, delete the file
    <span data-role="file">/etc/modprobe.d/xivo-wcb4xxp.conf</span>:
    
        rm /etc/modprobe.d/xivo-wcb4xxp.conf

4.  Then, apply the configuration by restarting the services:
    
        wazo-service restart

<div class="note">

<div class="admonition-title">

Note

</div>

Expected behavior:

  - The dahdi show status command should show the BRI spans in **OK**
    status even if there is no call,
  - In asterisk CLI you may see the spans going Up/Down/Up : it is *a
    problem* only if incoming or outgoing calls are rejected.

</div>

### PTP with layer1/layer2 persistence

In this mode we will configure asterisk and DAHDI:

  - to use Point-to-Point (PTP) signalling,
  - and use default behavior for Layer1 and Layer2.

Follow theses steps to configure:

1.  In file <span data-role="file">/etc/asterisk/chan\_dahdi.conf</span>
    remove all occurrences of `layer1_presence` and `layer2_persistence`
    options.

2.  In the file
    <span data-role="file">/etc/asterisk/dahdi-channels.conf</span> use
    `bri_cpe` signalling:
    
        signalling = bri_cpe

3.  If it exists, delete the file
    <span data-role="file">/etc/modprobe.d/xivo-wcb4xxp.conf</span>:
    
        rm /etc/modprobe.d/xivo-wcb4xxp.conf

4.  Then, apply the configuration by restarting the services:
    
        wazo-service restart

<div class="note">

<div class="admonition-title">

Note

</div>

Expected behavior:

  - The dahdi show status command should show the BRI spans in **OK**
    status even if there is no call,
  - In asterisk CLI you should not see the spans going Up and Down : if
    it happens, it is *a problem* only if incoming or outgoing calls are
    rejected.

</div>
