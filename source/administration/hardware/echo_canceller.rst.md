# Hardware Echo-cancellation

It is *recommended* to use telephony cards with an hardware
echo-canceller module.

<div class="warning">

<div class="admonition-title">

Warning

</div>

with **TE13x, TE23x and TE43x** cards, you **MUST** install the
echo-canceller firmware. Otherwise the card won't work properly.

</div>

## Know which firmware you need

If you have an hardware echo-canceller module you **have to** install
its firmware.

You first need to know which firmware you have to install. The simplest
way is to restart dahdi and then to lookup in the dmesg which firmware
does DAHDI request at startup:

    wazo-service restart
    dmesg |grep firmware
    [5461540.738209] wct4xxp 0000:01:0e.0: firmware: agent aborted loading dahdi-fw-oct6114-064.bin (not found?)
    [5461540.738310] wct4xxp 0000:01:0e.0: VPM450: firmware dahdi-fw-oct6114-064.bin not available from userspace

In the example above you can see that the module `wct4xxp` requested the
`dahdi-fw-oct6114-064.bin` firmware file but did not found it. But you
now know that you need the `dahdi-fw-oct6114-064.bin` firmware.

## Install the firmware

When you know which firmware you need you can install it with
`xivo-fetchfw` utility.

1.  Use `xivo-fetchfw` to find the name of the package. You can search
    for `digium` occurrences in the available packages:
    
        xivo-fetchfw search digium

2.  Find the package name which matches the firmware file you need. In
    our example, we need the `dahdi-fw-oct6114-064.bin` file which is
    supplied by the package named `digium-oct6114-064`:
    
        xivo-fetchfw install digium-oct6114-064

## Activate the Hardware Echo-cancellation

Now that you installed hardware echo-canceller firmware you must
activate it in
<span data-role="file">/etc/asterisk/chan\_dahdi.conf</span> file:

    echocancel = 1

## Apply the configuration

To apply the configuration, restart the services:

    wazo-service restart

## Next step

Now that you have loaded the correct module for your card you must:

1.  check if you need to follow one of the
    <span data-role="ref">echo\_can\_specific\_conf</span> sections
    below,
2.  and continue with the next configuration step which is to
    <span data-role="ref">configure your card
    \<card\_configuration\></span> according to the operator links.

## Specific configuration

This section describes some specific configuration. You should not
follow them unless you have a specific need.

### Use the Hardware Echo-canceller for DTMF detection

If you have an hardware echo-canceller you *may* want to use it to
detect the DTMF signal (instead of asterisk).

1.  Create the file
    <span data-role="file">/etc/modprobe.d/xivo-hwec-dtmf.conf</span>:
    
        touch /etc/modprobe.d/xivo-hwec-dtmf.conf

2.  Fill it with the following lines replacing `DAHDI_MODULE_NAME` by
    the correct module name (`wcte13xp`, `wct4xxp` ...):
    
        options DAHDI_MODULE_NAME vpmdtmfsupport=1

3.  Then, restart the services:
    
        wazo-service restart
