# Configuration Files

This section describes some of the Wazo configuration files.

## Configuration priority

Usually, the configuration is read from two locations: a configuration
file `config.yml` and a configuration directory `conf.d`.

Files in the `conf.d` extra configuration directory:

  - are used in alphabetical order and the first one has priority
  - are ignored when their name starts with a dot
  - are ignored when their name does not end with `.yml`

For example:

`.01-critical.yml`:

    log_level: critical

`02-error.yml.dpkg-old`:

    log_level: error

`10-debug.yml`:

    log_level: debug

`20-nodebug.yml`:

    log_level: info

The value that will be used for `log_level` will be `debug` since:

  - `10-debug.yml` comes before `20-nodebug.yml` in the alphabetical
    order.
  - `.01-critical.yml` starts with a dot so is ignored
  - `02-error.yml.dpkg-old` does not end with `.yml` so is ignored

## File configuration structure

Configuration files for every service running on a Wazo server will
respect these rules:

  - Default configuration directory in
    <span data-role="file">/etc/xivo-{{service}}/conf.d</span> (e.g.
    <span data-role="file">/etc/xivo-agentd/conf.d/</span>)
  - Default configuration file in
    <span data-role="file">/etc/xivo-{{service}}/config.yml</span> (e.g.
    <span data-role="file">/etc/xivo-agentd/config.yml</span>)

The files <span data-role="file">/etc/xivo-{{service}}/config.yml</span>
should not be modified because **they will be overridden during
upgrades**. However, they may be used as examples for creating
additional configuration files as long as they respect the
<span data-role="ref">configuration-priority</span>. Any exceptions to
these rules are documented below.

## wazo-auth

  - Default configuration directory:
    <span data-role="file">/etc/wazo-auth/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/wazo-auth/config.yml</span>

## xivo-agentd

  - Default configuration directory:
    <span data-role="file">/etc/xivo-agentd/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-agentd/config.yml</span>

## xivo-amid

  - Default configuration directory:
    <span data-role="file">/etc/xivo-amid/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-amid/config.yml</span>

## xivo-confgend

  - Default configuration directory:
    <span data-role="file">/etc/xivo-confgend/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-confgend/config.yml</span>
  - Default templates directory:
    <span data-role="file">/etc/xivo-confgend/templates</span>

## xivo-dao

  - Default configuration directory:
    <span data-role="file">/etc/xivo-dao/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-dao/config.yml</span>

This configuration is read by many Wazo programs in order to connect to
the Postgres database of Wazo.

## xivo-dird-phoned

  - Default configuration directory:
    <span data-role="file">/etc/xivo-dird-phoned/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-dird-phoned/config.yml</span>

## xivo-provisioning

  - Default configuration directory:
    <span data-role="file">/etc/xivo-provd/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-provd/config.yml</span>

## xivo-websocketd

  - Default configuration directory:
    <span data-role="file">/etc/xivo-websocketd/conf.d</span>
  - Default configuration file:
    <span data-role="file">/etc/xivo-websocketd/config.yml</span>

## xivo\_ring.conf

  - Path:
    <span data-role="file">/etc/xivo/asterisk/xivo\_ring.conf</span>
  - Purpose: This file can be used to change the ringtone played by the
    phone depending on the origin of the call.

<div class="warning">

<div class="admonition-title">

Warning

</div>

Note that this feature has not been tested for all phones and all call
flows. This page describes how you can customize this file but does not
intend to list all validated call flows or phones.

</div>

This file <span data-role="file">xivo\_ring.conf</span> consists of :

  - profiles of configuration (some examples for different brands are
    already included: `[aastra]`, `[snom]` etc.)
  - one section named `[number]` where you apply the profile to an
    extension or a context etc.

Here is the process you should follow if you want to use/customize this
feature :

1.  Create a new profile, e.g.:
    
        [myprofile-aastra]

2.  Change the `phonetype` accordingly, in our example:
    
        [myprofile-aastra]
        phonetype = aastra

3.  Chose the ringtone for the different type of calls (note that the
    ringtone names are brand-specific):
    
        [myprofile-aastra]
        phonetype = aastra
        intern = <Bellcore-dr1>
        group = <Bellcore-dr2>

4.  Apply your profile, in the section `[number]`

>   - to a given list of extensions (e.g. 1001 and 1002):
>     
>         1001@default = myprofile-aastra
>         1002@default = myprofile-aastra
> 
>   - or to a whole context (e.g. default):
>     
>         @default = myprofile-aastra

5.  Restart `xivo-agid` service:
    
        service xivo-agid restart

## Asterisk configuration files

Asterisk configuration files are located at /etc/asterisk. These files
are packaged with Wazo and you should not modify files that are located
at the root of this directory.

To add you own configurations, you must add a new configuration file in
the corresponding .d directory.

For example, if you need to add a new user to the manager.conf
configuration file, you would add a new file
/etc/asterisk/manager.d/my\_new\_user.conf with the following content:

    .. code-block: ini

> \[my\_new\_user\] secret=v3ry5ecre7 deny=0.0.0.0/0.0.0.0
> permit=127.0.0.1/255.255.255.0 read = system

The same logic applies to all Asterisk configuration files except
asterisk.conf and modules.conf.
